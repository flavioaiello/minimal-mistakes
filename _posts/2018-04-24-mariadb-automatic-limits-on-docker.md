When deploying the official docker hub mariadb image, an unspecific default configuration file will be provided, thus no performance parameters are set nor the container limits are taken into account. The result is poor database performance and many issues if not mitigated properly.

Based on the official images, the custom image as outlined below is recommended for a mariadb connection pool tandem with the great [Hikari](https://github.com/brettwooldridge/HikariCP) library reaching a basic performance level.

## [Hikari](https://github.com/brettwooldridge/HikariCP)
The mariadb `wait_timeout` parameter is limited by the swarm overlay network constraint of terminating tcp connections after 15 minutes. In order to set the hikari `maxLifetime` parameter to 10 minutes, the `wait_timeout` parameter must be increased from `600` to `750` seconds as shown above.

## Swarm
When using overlay networking with Docker swarm mode or other orchestrators, the ip address of containers will be provisioned on a short term basis. By default a lookup on every request is made and cached. Disabling both, `skip-host-cache` and `skip-name-resolve` improves the performance of each query.

## Example Dockerfile
The Dockerfile excerpt below can be found on Github as referenced in the footer.
```
...
RUN sed -re 's/^(bind-address|log|user)/#&/' \
    -e '/wait_timeout[^_]\s*/c\wait_timeout = 750' \
    -e '/\[mysqld\]/a skip-host-cache' \
    -e '/\[mysqld\]/a skip-name-resolve' \
    -i /etc/mysql/my.cnf
...
```

## Container limits
Taking container limits into account and calculating on startup buffer and cache size parameters, leads to a vastly performance improvement. `AWK` is the simplest command available to do the math based on the cgroup parameters, calculating the required integers as shown below:

## Example entrypoint.sh
The entrypoint excerpt below can be found on Github as referenced in the footer.
```
...
sed -e "/innodb_buffer_pool_size[^_]\s*/c\innodb_buffer_pool_size = $(awk '{ print int($1*3/4)}' /sys/fs/cgroup/memory/memory.limit_in_bytes)" \
    -e "/tmp_table_size[^_]\s*/c\tmp_table_size = $(awk '{ print int($1/16)}' /sys/fs/cgroup/memory/memory.limit_in_bytes)" \
    -e "/max_heap_table_size[^_]\s*/c\max_heap_table_size = $(awk '{ print int($1/16)}' /sys/fs/cgroup/memory/memory.limit_in_bytes)" \
    -e "/query_cache_limit[^_]\s*/c\query_cache_limit = $(awk '{ print int($1/6000)}' /sys/fs/cgroup/memory/memory.limit_in_bytes)" \
    -e "/query_cache_size[^_]\s*/c\query_cache_size = $(awk '{ print int($1/12)}' /sys/fs/cgroup/memory/memory.limit_in_bytes)" \
    -e "/slow_query_log[^_]\s*/c\slow_query_log = ${SLOW_QUERY_LOG:-1}" \
    -e "/long_query_time[^_]\s*/c\long_query_time = ${LONG_QUERY_TIME:-5}" \
    -i /etc/mysql/my.cnf
...
```

The above sources can be found on Github and the custom mariadb docker images on Docker Hub.
