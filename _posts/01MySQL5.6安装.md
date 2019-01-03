# MySQL 5.6 二进制安装方式

## 	MySQL 5.6 下载

​		![](E:\gif_rec\mysql5.6下载.gif)

​												<!--图一-->

​		进入**MySQL**[官网](https://www.mysql.com/)

​		选择**DOWNLOAD**

​		将页面拉到最后选择**MySQL Community Edition（GPL）**中的 —> **Community（GPL）Downloads**

​		选择**MySQL Community Server（GPL）**

​		选择**Looking for previous GA versions** 中的 —> **MySQL Community Server 5.6**

​		**Selection Version** 选择 **5.6.42**

​		**Selection Version** 选择 **Linux-Generic**

​		**Select OS Version** 选择 **Linux-Generic（glibc 2.12）（x86，64bit）**

​		选择**Download**

------



## 	解压MySQL 5.6 二进制安装包

![mysql5.6解压](E:\gif_rec\mysql5.6解压.gif)

​												<!--图二-->			

```shell
[root@localhost ~]# cd /usr/local/src
[root@localhost src]# ll
total 321272
-rw-r--r-- 1 root root 328979165 Jan  3 08:27 mysql-5.6.42-linux-glibc2.12-x86_64.tar.gz
[root@localhost src]# tar -zxvf mysql-5.6.42-linux-glibc2.12-x86_64.tar.gz
[root@localhost src]# mv mysql-5.6.42-linux-glibc2.12-x86_64 /usr/local/mysql
```

------

## 	

## 	创建目录并赋权

```shell
[root@localhost ~]# mkdir -p /data/data
[root@localhost ~]# mkdir -p /data/logs
[root@localhost ~]# useradd mysql
[root@localhost ~]# chown -R mysql:mysql /data
[root@localhost ~]# chown -R mysql:mysql /usr/local/mysql
```

------

## 	

## 	编辑参数文件

```shell
[root@localhost ~]# vim /etc/my.cnf

[client]
port            = 3306
socket          = /tmp/mysql3306.sock
secure_auth     = false

[mysqld]
port            = 3306
socket          = /tmp/mysql3306.sock
datadir         = /data/data
#read_only       = on

#--- GLOBAL ---#
character_set_server    = utf8
lower_case_table_names  = 1
log-output              = FILE
log-error               = /data/logs/mysql-error.log
#general_log
general_log_file        = /data/logs/mysql.log
pid-file                = /data/data/mysql.pid
slow-query-log          = 1
slow_query_log_file     = /data/logs/mysql-slow.log
tmpdir                  = /tmp/
long_query_time         = 2
innodb_force_recovery   = 0
#innodb_buffer_pool_dump_at_shutdown = 1
#innodb_buffer_pool_load_at_startup = 1
#--------------#

#thread_concurrency      = 8  
thread_cache_size       = 51  
table_open_cache        = 16384
open_files_limit        = 65535
table_definition_cache  = 16384
sort_buffer_size        = 2M
join_buffer_size        = 2M
read_buffer_size        = 2M
read_rnd_buffer_size    = 8M
key_buffer_size         = 32M
bulk_insert_buffer_size = 16M
myisam_sort_buffer_size = 64M
tmp_table_size          = 32M
max_heap_table_size     = 16M 
query_cache_size       = 32MB

#gtid_mode=on
#log_slave_updates=1
#enforce_gtid_consistency=1

#--- NETWORK ---#
back_log                = 103
max-connections         = 512
max_connect_errors      = 100000
max_allowed_packet      = 32M
interactive_timeout     = 600
wait_timeout            = 600
skip-external-locking
#max_user_connections    = 0
external-locking        = FALSE
#skip-name-resolve

#--- REPL ---#
server-id               = 1001093306
sync_binlog             = 1
log-bin                 = mysql-bin
binlog_format           = row
expire_logs_days        = 10
relay-log               = relay-log
replicate-ignore-db     = test
log_slave_updates       =1
#skip-slave-start
binlog_cache_size       =4M
max_binlog_cache_size   =8M
max_binlog_size         =1024M



#--- INNODB ---#
default_storage_engine          = InnoDB
innodb_data_file_path           = ibdata1:1024M:autoextend
innodb_buffer_pool_size         = 1040M
innodb_buffer_pool_instances    = 1
innodb_additional_mem_pool_size = 16M
innodb_log_files_in_group       = 2
innodb_log_file_size            = 256MB
innodb_log_buffer_size          = 16M
innodb_flush_log_at_trx_commit  = 2
innodb_lock_wait_timeout        = 30
innodb_flush_method             = O_DIRECT
innodb_max_dirty_pages_pct      = 75
innodb_io_capacity              = 200
innodb_thread_concurrency       = 32
innodb_open_files               = 65535
innodb_file_per_table           = 1
transaction_isolation           = REPEATABLE-READ
innodb_locks_unsafe_for_binlog  = 0
#innodb_purge_thread            = 4
skip_name_resolve               = 1
[mysqldump]
quick
max_allowed_packet = 32M


[mysql]
auto-rehash
# Remove the next comment character if you are not familiar with SQL
#safe-updates
default_character_set=utf8


[mysqlhotcopy]
interactive-timeout
```

------



## 	安装依赖包

```shell
[root@localhost ~]# yum -y install *Dumper*
```

------



## 	初始化MySQL

```shell
[root@localhost ~]# /usr/local/mysql/scripts/mysql_install_db --defaults-file=/etc/my.cnf --basedir=/usr/local/mysql --database=/data/data --user=mysql
```

------



## 	设置环境变量

```shell
[root@localhost ~]# su - mysql
[mysql@localhost ~]$ vim .bash_profile

# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

MYSQL_HOME=/usr/local/mysql

PATH=$MYSQL_HOME/bin:$PATH:$HOME/.local/bin:$HOME/bin

export PATH
~                                                                      
~                                                                      
~                                                                      
~                                                                      
~                                                                      
~                                                                      
~                                                                      
~                                                                      
~                                                                      
~                                                                      
~                                                                      
~                                                                      
:wq

[mysql@localhost ~]$ . .bash_profile
```

------



## 	启动MySQL服务

```shell
[mysql@localhost ~]$ mysqld_safe &
```

------



## 	登录MySQL

```shell
[mysql@localhost ~]$ mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.6.42-log MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>

```

------



## 	设置密码

```mysql
mysql> set password=password('root');
mysql> flush privileges;
```

------



## 	修改权限

```mysql
mysql> grant all on *.* to 'root'@'%' identified by 'root';
mysql> flush privileges;
```

