Not really a howto here but more a collection of tips and tricks on
MySQL

Disable schema performance
================================

Edit the file /etc/mysql/mysql.conf.d/mysqld.cnf and add in:

    [Mysqld]
    performance_schema = OFF

Optimize MySQL
===============

> ** Important **
>
> This method is at your own risk. If there are any problems
> support will not be possible.

-   Stop the MySQL daemon and delete the log files:

<! - ->

    mysql stop service
    rm / var / lib / mysql / ib_logfile *

-   Then do:

<! - ->

    touch /etc/mysql/conf.d/jeedom_my.cnf
    echo "[mysqld]" >> /etc/mysql/conf.d/jeedom_my.cnf
    echo "key_buffer_size = 16M" >> /etc/mysql/conf.d/jeedom_my.cnf
    echo "thread_cache_size = 16" >> /etc/mysql/conf.d/jeedom_my.cnf
    echo "tmp_table_size = 48M" >> /etc/mysql/conf.d/jeedom_my.cnf
    echo "max_heap_table_size = 48M" >> /etc/mysql/conf.d/jeedom_my.cnf
    echo "query_cache_type = 1" >> /etc/mysql/conf.d/jeedom_my.cnf
    echo "query_cache_size = 16M" >> /etc/mysql/conf.d/jeedom_my.cnf
    echo "query_cache_limit = 2M" >> /etc/mysql/conf.d/jeedom_my.cnf
    echo "query_cache_min_res_unit = 3K" >> /etc/mysql/conf.d/jeedom_my.cnf
    echo "innodb_flush_method = O_DIRECT" >> /etc/mysql/conf.d/jeedom_my.cnf
    echo "innodb_flush_log_at_trx_commit = 2" >> /etc/mysql/conf.d/jeedom_my.cnf
    echo "innodb_log_file_size = 32M" >> /etc/mysql/conf.d/jeedom_my.cnf

-   Restart MySQL:

<! - ->

    mysql start service
