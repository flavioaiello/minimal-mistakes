Building software sources and their according runtime containers are two different concerns. Docker introduced about one year ago the multistage build feature, improving vastly the developer experience. Multistage build leaves security keys and build specific packages behind and focuses on small and secure production runtime containers.

How CI/CD systems handled their propiertary build approach becomes now also portable. Build reports can simply be exposed by mounting volumes to the build system workspace, thus offering the same level of usability as usual.

## Examples
Below we see two different examples how to accomplish multistage building.

### Java
```
### Source build - the following "build" label is used in the runtime stage below
FROM maven:3-jdk-8-alpine as build
COPY build /build
WORKDIR /build
RUN set -ex ;\
    echo "*** Build source ***" ;\
    mvn -B clean install

### Runtime build
FROM alpine:3.7
# Packages
RUN set -ex ;\
    apk update ;\
    apk upgrade ;\
    apk add --no-cache su-exec openjdk8-jre ;\
    rm -rf /var/cache/apk/* ;\
    echo "*** appuser system account ***" ;\
    addgroup -S appuser ;\
    adduser -S -D -h /home/appuser -s /bin/false -G appuser -g "appuser system account" appuser ;\
    chown -R appuser /home/appuser
# Copy build artefacts to runtime container image - "from build" means the stage above
COPY --from=build /build/app /usr/local/bin
# Copy local files to runtime container image
COPY files /
# Default port exposure
EXPOSE 8080
# Init and PID 1 execution
ENTRYPOINT ["entrypoint.sh"]
CMD ["java","/usr/local/bin/myapp.jar"]
```
### Golang
```
### Source build - the following "build" label is used in the runtime stage below
FROM golang:1.10 as build
COPY build /build
WORKDIR /build
RUN go get -d -v -t;\
    CGO_ENABLED=0 GOOS=linux go build -v -o /usr/local/bin/serve

### Runtime build - based on empty base image without to switch user
FROM scratch
# Copy build artefacts to runtime container image - "from build" means the stage above
COPY --from=build /usr/local/bin/serve /usr/local/bin/
# Copy local files to runtime container image
COPY files /
WORKDIR /wwwroot
EXPOSE 8080
CMD ["/usr/local/bin/serve", "-p", "8080", "-d", "/wwwroot"]
```
## Automation
When using the multistage build feature with Jenkins 2.x, the main concern of the Jenkinsfile in each repository remains the Dockerfile build.
```
#!groovy

def dockerImageName = env.JOB_NAME.substring(env.JOB_NAME.lastIndexOf('/') + 1)
def dockerRegistry = 'https://myregistry'
def dockerRepository = 'myrepo'
def dockerCredentialsId = 'myuser'

node {
    stage('Checkout') {
        checkout scm
    }

    def dockerImageTag = sh(returnStdout: true, script: 'git describe --all').trim().replaceAll(/(.*\/)?(.+)/,'$2')
    def pushAsLatest = (dockerImageTag ==~ /v(\d+.\d+.\d+)/)

    stage('Build and Push') {
        docker.withRegistry(dockerRegistry, dockerCredentialsId) {
            // Set repository and image name
            def image = docker.build dockerRepository + "/" + dockerImageName, "--build-arg TAG=${dockerImageTag} ."
            // Push actual tag
            image.push(dockerImageTag)
            // Push latest tag if it's a release
            if (pushAsLatest) {
                image.push('latest')
            }
            echo "*** Container image successfully pushed to registry. ***"
        }
    }

    stage('Cleanup') {
        sh 'docker system prune -f'
    }  
}

```
