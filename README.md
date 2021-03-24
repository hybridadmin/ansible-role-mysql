## mysql
[![CI](https://github.com/hybridadmin/ansible-role-mysql/actions/workflows/build.yml/badge.svg?branch=main)](https://github.com/hybridadmin/ansible-role-mysql/actions/workflows/build.yml)
![Ansible Role](https://img.shields.io/ansible/role/d/53652)
![Ansible Quality Score](https://img.shields.io/ansible/quality/53652)

> A role to install standalone or clustered mysql/mariadb servers on supported distros.

The galera cluster options depending on whether `mysql` or `mariadb` is required are as below:

* [`Galera Cluster for MySQL`](https://galeracluster.com/library/documentation/install-mysql.html)
* [`MariaDB Galera Cluster`](https://galeracluster.com/library/documentation/install-mariadb.html)


## Requirements

The following is required on the ansible host:
* Ansible Collections:
  * [`community.mysql`](https://github.com/ansible-collections/community.mysql)
* Python packages:
  * [`pymsql`](https://pypi.org/project/PyMySQL/)

## Role Variables

The variables that can be set are listed below and default values are provided in [`defaults/main.yml`](defaults/main.yml):

    mysql_install_settings:
      use_mariadb: false
      version: 8.0
      root_password: ''

The instance installation settings that are used for either `mysql` or `mariadb` are specified here. `use_mariadb` is used to choose between
`mysql` or `mariadb`. `version` specifies the `mysql` or `mariadb` version to be installed ( i.e mysql `5.6`+ or mariadb `10.3`+)


    mysql_conf_settings:
      port: 3306
      bind_address: "127.0.0.1"
      datadir: "/var/lib/mysql/"
      socket: "/var/run/mysqld/mysqld.sock"
      pid_file: "/var/run/mysqld/mysqld.pid"
      skip_name_resolve: false
      sql_mode: ''
      log_error: ''
      slow_query_log_file: '/var/log/mysql/mysql-slow.log'
      slow_query_time: 2
      key_buffer_size: "256M"
      max_allowed_packet: "64M"
      table_open_cache: 256
      sort_buffer_size: "1M"
      read_buffer_size: "1M"
      read_rnd_buffer_size: "4M"
      myisam_sort_buffer_size: "64M"
      thread_cache_size: 8
      query_cache_type: 0
      query_cache_size: "16M"
      query_cache_limit: "1M"
      max_connections: 150
      tmp_table_size: "16M"
      max_heap_table_size: "16M"
      group_concat_max_len: 1024
      join_buffer_size: 262144
      wait_timeout: 28800
      lower_case_table_names: 0
      event_scheduler_state: "OFF"
      binlog_format: "ROW"
      innodb_file_per_table: 1
      innodb_buffer_pool_size: "1G"
      innodb_log_file_size: "256M"
      innodb_log_buffer_size: "128M"
      innodb_flush_log_at_trx_commit: 1
      innodb_lock_wait_timeout: 50
      innodb_flush_method: "O_DIRECT"
      innodb_autoinc_lock_mode: 2
      innodb_thread_concurrency: 0
      innodb_stats_on_metadata: "OFF"
      innodb_buffer_pool_instances: 1

The configuration settings for the `mysql`/`mariadb` instance are specifed here. These settings will be saved to `/etc/mysql/conf.d/server.cnf` on `debian` and `/etc/my.cnf.d/server.cnf` on `redhat` distros.


    galera_cluster_settings:
      enabled: true
      wsrep_sst_method: rsync
      wsrep_provider_options: "gcache.size=300M; gcache.page_size=300M"
      wsrep_cluster_name: "GaleraCluster"
      wsrep_sst_user: "mariabackup"
      wsrep_sst_pass: "tstwytw22211"

The `galera` cluster configuration settings are specified here.



## Dependencies

None.

## Example Playbook

### Standalone MySQL 8.0 server:

```yaml
    - hosts: servers
      vars:
        mysql_install_settings:
          use_mariadb: false
          version: 8.0
      roles:
         - { role: hybridadmin.mysql }
```

### Standalone MariaDB 10.4 server:

```yaml
    - hosts: servers
      vars:
        mysql_install_settings:
          use_mariadb: true
          version: 10.4
      roles:
         - { role: hybridadmin.mysql }
```

### MySQL Galera Cluster (MySQL 5.7):

```yaml
    - hosts: servers
      vars:
        mysql_install_settings:
          use_mariadb: false
          version: 5.7
        galera_cluster_settings:
          enabled: true
          wsrep_sst_method: rsync
      roles:
         - { role: hybridadmin.mysql }
```

### MariaDB Galera cluster (MariaDB 10.4):

```yaml
    - hosts: servers
      vars:
        mysql_install_settings:
          use_mariadb: true
          version: 10.4
        galera_cluster_settings:
          enabled: true
          wsrep_sst_method: rsync
      roles:
         - { role: hybridadmin.mysql }
```


## License

[`Apache License 2.0`](./LICENSE)


## Author Information

Created by [`hybridadmin`](https://github.com/hybridadmin)
