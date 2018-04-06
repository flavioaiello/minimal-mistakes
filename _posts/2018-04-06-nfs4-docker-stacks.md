Volumes are required whenever the container state must be preserved between deployments. There are plenty of blog posts out there about Docker Volumes and also the recommended [Docker Docs](https://docs.docker.com/storage/volumes/) sites.

When it comes to Docker Stacks and NFS4, there is much confusion. The `docker-compose.yml` file below is the result of few hours of research, as it seems to be undocumented. Starting with `Version: 3.2`, the local driver supports type `NFS` and `NFS4` without any further installation of volume plugins. The default mount options like `rw`, `hard` etc. seem to be set during the mount operation. This can be checked on the host with the `mount` command.

Failure of NFS seem to be tollerated by Docker without any complaints. As soon NFS is reachable all services return to work as expected. Make sure to create the mount point in advance, as it is required on startup for the mount operation.

## Docker Sample Stack

```
Version: '3.2'

services:

  mariadb:
    image: mariadb:latest
    volumes:
      - type: volume
        source: mariadb
        target: /var/lib/mysql/
        volume:
          nocopy: true
    ...

volumes:
  mariadb:
    driver: "local"
    driver_opts:
      type: "nfs4"
      o: "addr=192.168.1.100"
      device: "192.168.1.100:/nfs_data/${CONTEXT}/databases/mariadb"
...
```
The statement `docker stack deploy -c docker-compose.yml mystack` starts your stack as usual and creates the named volume as expected on every required node. If a container will be rescheduled on another node, then Swarm will take care of the volume, eg. will recreate the volume mount on the other node.

```
$ docker volume ls
DRIVER              VOLUME NAME
local               mystack_mariadb
```

## Permission issues with NFS

Any service accessing NFS to persist their state, needs to reset the file ownership during startup, so that the container can be started with the service account instead of root. For this reason, the NFS share should be exposed with the ´no_root_squash´ enabled, so that remote root users are able to change any file on the shared file system.
