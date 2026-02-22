---
title: "ProxySQL + Percona XtraDB Cluster Using Docker"
date: 2018-07-20
slug: "proxysql-percona-xtradb-cluster-using-docker"
categories: ['MySQL', 'DevOps', 'Docker']
tags: ['ProxySQL', 'Percona', 'XtraDB', 'MySQL', 'Docker', 'Load Balancing', 'PMM', 'Grafana']
description: "介紹如何使用 Docker 搭建 ProxySQL 搭配 Percona XtraDB Cluster，涵蓋讀寫分離、負載均衡、Query Cache、Galera 支援及 PMM 監控設定。"

---

GitHub:

* Percona Xtradb Cluster https://github.com/percona/percona-xtradb-cluster
* Proxysql https://github.com/sysown/proxysql
* proxsysql-admin-tool https://github.com/percona/proxysql-admin-tool
* percona dockers https://github.com/percona/percona-docker

參考：

* https://github.com/sysown/proxysql/wiki/ProxySQL-Configuration

* http://seanlook.com/2017/04/10/mysql-proxysql-install-config/

延伸閱讀

* https://www.percona.com/doc/percona-xtradb-cluster/LATEST/howtos/proxysql.html

  官方文件的 load balancing，但是透過 proxysql-admin-tool 來操作

* https://www.cnblogs.com/f-ck-need-u/p/9300829.html
* ProxySQL之性能測試對比 http://seanlook.com/2017/04/20/mysql-proxysql-performance-test/
* Percona監控工具初探 https://www.hi-linux.com/posts/52766.html
* ProxySQL 安裝配置詳解及讀寫分離、負載均衡 https://dwj999.github.io/ProxySQL-%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE%E8%AF%A6%E8%A7%A3%E5%8F%8A%E8%AF%BB%E5%86%99%E5%88%86%E7%A6%BB%E3%80%81%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1.html



proxysql.cnf 只會安裝完後第一次啟動時被用到，啟動後就會把 cnf 的資訊讀到 `/var/lib/proxysql/proxysql.db` 裡，之後透過 mysql client 的修改也都是寫到該 db 裡。

而 proxysql-admin-tool 的使用時機則是在第一次啟動後，協助建立相關的設定（如使用者設定、singlewrite/readwrite 模式）

proxysql-admin-tool 使用前，需要先將 proxysql 啟動，可以使用預設的 proxysql.cnf，但建議把 admin credential 改掉後再啟動，啟動完畢後，修改 proxysql-admin.cnf。

可以不需要使用 proxysql-admin-tool，只透過 proxysql.cnf 也是可以用的。



在 local build，使用到以下檔案

* pxc-57 docker https://github.com/percona/percona-docker/tree/master/pxc-57
* proxysql docker https://github.com/percona/percona-docker/tree/master/proxysql
  * 可以設定 add_to_cluster.sh，調整需要的參數



writer_hostgroup = 1

reader_hostgroupd = 2



1. 先建立 etcd

2. 先在 local 建立 pxc 搭配剛剛建立的 etcd

3. 建立 proxysql，並掛上 pxc 

4. 調整 proxysql

   1. read/write 分離

      參考 https://github.com/sysown/proxysql/wiki/ProxySQL-Configuration#mysql-replication-hostgroups

      若要設定 read/write 分離，則想要只有 read 的機器都需要開啟 `read_only`

      ```
      set global read_only = 1;
      SHOW GLOBAL VARIABLES LIKE 'read_only';
      ```

      並在 `mysql_replication_hostgroups` table 設定相關資訊

      ```
      Admin> SHOW CREATE TABLE mysql_replication_hostgroups\G
      *************************** 1. row ***************************
             table: mysql_replication_hostgroups
      Create Table: CREATE TABLE mysql_replication_hostgroups (
      writer_hostgroup INT CHECK (writer_hostgroup>=0) NOT NULL PRIMARY KEY,
      reader_hostgroup INT NOT NULL CHECK (reader_hostgroup<>writer_hostgroup AND reader_hostgroup>0),
      comment VARCHAR,
      UNIQUE (reader_hostgroup))
      1 row in set (0.00 sec)

      Admin> INSERT INTO mysql_replication_hostgroups (reader_hostgroup, writer_hostgroup, comment) VALUES (2,1,'cluster');
      Query OK, 1 row affected (0.00 sec)

      # 確認
      Admin> select * from mysql_replication_hostgroups;
      ```

      透過觀察 query log，再新增 mysql rules 

      ```
      # query log
      Admin> SELECT hostgroup hg, sum_time, count_star, digest, digest_text FROM stats_mysql_query_digest ORDER BY sum_time DESC;

      Admin> INSERT INTO mysql_query_rules (rule_id,active,username,match_digest,destination_hostgroup,apply) VALUES (10,1,'go','^INSERT',1,1);

      # 或是使用 digest 來新增
      Admin> INSERT INTO mysql_query_rules (rule_id,active,username,match_digest,destination_hostgroup,apply) VALUES (10,1,'go','0x86CFF744679FA77D',1,1); # 0x86CFF744679FA77D = insert ...

      # on update 要用 writer
      Admin> INSERT INTO mysql_query_rules (rule_id,active,username,match_digest,destination_hostgroup,apply) VALUES (12,1,'go','^SELECT .* ON UPDATE',1,1);

      # select 要用 reader
      Admin> INSERT INTO mysql_query_rules (rule_id,active,username,match_digest,destination_hostgroup,apply) VALUES (11,1,'go','^SELECT id, oid FROM identities WHERE oid=\? ORDER BY ID desc LIMIT \?$',2,1);

      Admin> INSERT INTO mysql_query_rules (rule_id,active,match_digest,destination_hostgroup,apply)
      VALUES
      (1,1,'^SELECT.*FOR UPDATE$',10,1),
      (2,1,'^SELECT',20,1);

      # 確認
      Admin> SELECT match_digest,match_pattern,destination_hostgroup FROM mysql_query_rules WHERE active=1 ORDER BY rule_id;

      # load to runtime and save to disk
      Admin> LOAD MYSQL QUERY RULES TO RUNTIME; SAVE MYSQL QUERY RULES TO DISK;
      ```

      

      壓測後可以透過以下指令確認是否有分流

      ```
      # 先 reset counter
      Admin> SELECT * FROM stats_mysql_query_digest_reset LIMIT 1;

      # 再觀察 summary
      Admin> SELECT hostgroup hg, SUM(sum_time), SUM(count_star) FROM stats_mysql_query_digest GROUP BY hostgroup;
      ```

      如果沒有，確認 mysql_users 裡的 transaction_persistent 是不是被設定 1 了，若被設定成 1，記得改成 0。

      https://github.com/sysown/proxysql/issues/1203

      

      

   2. load balancer

      參考 https://www.digitalocean.com/community/tutorials/how-to-use-proxysql-as-a-load-balancer-for-mysql-on-ubuntu-16-04

      ```
      # XXXX
      mysql_replication_hostgroups
      SHOW CREATE TABLE mysql_replication_hostgroups\G
      SHOW CREATE TABLE s\G

      INSERT INTO mysql_replication_hostgroups VALUES (1,2,'cluster1');
      ```

      ```
      Admin> INSERT INTO mysql_group_replication_hostgroups (writer_hostgroup, backup_writer_hostgroup, reader_hostgroup, offline_hostgroup, active, max_writers, writer_is_also_reader, max_transactions_behind) VALUES (1, 4, 2, 3, 1, 3, 1, 100);

      # 確認
      Admin> select * from mysql_group_replication_hostgroups;
      ```

      * `active` set to `1` enables ProxySQL's monitoring of these host groups.
      * `max_writers` defines how many nodes can act as writers. We used `3` here because In a multi-primary configuration, all nodes can be treated equal, so here we used `3` (the total number of nodes).
      * `writer_is_also_reader` set to `1` instructs ProxySQL to treat writers as readers as well.
      * `max_transactions_behind` sets the maximum number of delayed transactions before a node is classified as **offline**.

      再分別加入或是修改 `hostgroup_id` 為 `2` (writer_hostgroup)，同上面的設定

      ```
      Admin> INSERT INTO mysql_servers(hostgroup_id, hostname, port) VALUES (2, 'pxc-{1,2,3}', 3306);

      # 確認
      Admin> select * from mysql_servers;
      ```

      

      最後把所有的設定寫入 runtime 及 disk (sqlitedb)

      ```
      Admin> LOAD MYSQL QUERY RULES TO RUNTIME; SAVE MYSQL QUERY RULES TO DISK;
      Admin> LOAD MYSQL VARIABLES TO RUNTIME; SAVE MYSQL VARIABLES TO DISK;
      Admin> LOAD MYSQL SERVERS TO RUNTIME; SAVE MYSQL SERVERS TO DISK;
      ```

      再檢查一次 runtime 的 mysql_server

      ```
      Admin> SELECT hostgroup_id, hostname, status FROM runtime_mysql_servers;
      ```

   3. query cache

      ```
      Admin> INSERT INTO mysql_query_rules(active,match_pattern,destination_hostgroup,cache_ttl,apply) VALUES(1,'^SELECT id, oid FROM identities',2,2000,1);

      Admin> LOAD MYSQL QUERY RULES TO RUNTIME; SAVE MYSQL QUERY RULES TO DISK;
      ```





列出 query log，並使用總和時間排序

```
Admin> SELECT hostgroup hg, sum_time, count_star, digest_text FROM stats_mysql_query_digest ORDER BY sum_time DESC;
```





## Adding Galera Support

https://www.percona.com/doc/percona-xtradb-cluster/LATEST/howtos/proxysql.html

```
Admin> INSERT INTO scheduler(id,active,interval_ms,filename,arg1,arg2,arg3,arg4,arg5)
  VALUES (1,'1','10000','/usr/bin/proxysql_galera_checker','1','2','0','1',
  '/var/lib/proxysql/proxysql_galera_checker.log');
```

| Argument | Name                  | Required | Description                                                  |
| -------- | --------------------- | -------- | ------------------------------------------------------------ |
| `arg1`   | `HOSTGROUP WRITERS`   | YES      | The ID of the hostgroup with nodes that will server writes.  |
| `arg2`   | `HOSTGROUP READERS`   | NO       | The ID of the hostgroup with nodes that will server reads.   |
| `arg3`   | `NUMBER WRITERS`      | NO       | Maximum number of the node from the writer hostgroup that can be marked `ONLINE`. If set to `0`, all nodes can be marked `ONLINE`. |
| `arg4`   | `WRITERS ARE READERS` | NO       | If set to `1` (default), `ONLINE` nodes in the writer hostgroup will prefer not to be `ONLINE` in the reader `hostgroup`. |
| `arg5`   | `LOG FILE`            | NO       | File where node state checks and changes are logged to (verbose). |

```
Admin> LOAD SCHEDULER TO RUNTIME;
Admin> SELECT * FROM runtime_scheduler\G
Admin> SELECT hostgroup_id,hostname,port,status FROM mysql_servers;
```









新增 proxysql -> mysql 的使用者

```
mysql@pxc > CREATE USER 'proxyuser'@'%' IDENTIFIED BY 'proxypass';
mysql@pxc > GRANT ALL ON *.* TO 'proxyuser'@'%';

mysql@proxysql> INSERT INTO mysql_users (username,password) VALUES ('proxyuser','proxypass');
mysql@proxysql> LOAD MYSQL USERS TO RUNTIME; SAVE MYSQL USERS TO DISK;
```





## PMM (Percona Monitoring and Management)

ref:

* doc https://www.percona.com/doc/percona-monitoring-and-management/index.html

* https://www.hi-linux.com/posts/52766.html



安裝 server (docker)：

```shell
$ docker create \
   -v /opt/prometheus/data \
   -v /opt/consul-data \
   -v /var/lib/mysql \
   -v /var/lib/grafana \
   --name pmm-data \
   percona/pmm-server:1.1.1 /bin/true
$ docker run -d \
      -p 80:80 \
      --volumes-from pmm-data \
      --name pmm-server \
      --restart always \
      percona/pmm-server:1.1.1
```

安裝 client：

```
$ wget https://www.percona.com/downloads/pmm/1.12.0/binary/debian/jessie/x86_64/pmm-client_1.12.0-1.jessie_amd64.deb \
    && dpkg -i pmm-client_1.12.0-1.jessie_amd64.deb
```

設定 client：

```shell
$ pmm-admin config --server {PMM IP} --bind-address {client IP} --client-address {client IP}
OK, PMM server is alive.

PMM Server      | {PMM IP}
Client Name     | {client hostname}
Client Address  | {client IP}

# 設定 mysql
$ pmm-admin add mysql --user root --password xxx --host 127.0.0.1 --port 3306
```

接著就可以連上 PMM 看各種數據。

note: 想知道 container 的 ip 可以透過此指令來找到

```
$ docker inspect {container id} | grep IPAddress
```





## Grafana dashboard

https://github.com/percona/grafana-dashboards/blob/master/dashboards/ProxySQL_Overview.json



## Mysql status

除非有連線，不然 status 不會自動改

舉例：將 proxysql 開起來，pxc 沒有開，此時 runtime_mysql_servers 的 status 都還是 ONLINE

但透過 mysql client 試著連進去，會 timeout，此時 runtime_mysql_servers 的 status 就會是 SHUNNED



## centos 開機順序

舊版的會吃 /etc/rc.local，但其實是相容模式，背後還是透過 systemd 處理

若要在 rc.local 執行前啟動 docker 的話，則要在編輯 service 檔案 

https://fedoramagazine.org/systemd-unit-dependencies-and-order/

```
$ systemctl edit --full rc-local
Wants=docker.service
After=network.target docker.service

# 檢查 /etc/systemd/system/rc-local.service
```



??? 在 After 加入 docker.service，這樣就會在開機啟動時，執行 rc.local 前啟動 docker.service

```
[root@proxysql ~]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31

[root@proxysql ~]# vim /etc/sysconfig/selinux
```



## default proxysql.cnf

```
#file proxysql.cfg

# This config file is parsed using libconfig , and its grammar is described in:
# http://www.hyperrealm.com/libconfig/libconfig_manual.html#Configuration-File-Grammar
# Grammar is also copied at the end of this file



datadir="/var/lib/proxysql"

admin_variables=
{
	admin_credentials="admin:admin"
	mysql_ifaces="0.0.0.0:6032"
	refresh_interval=2000
#	debug=true
}

mysql_variables=
{
	threads=4
	max_connections=2048
	default_query_delay=0
	default_query_timeout=36000000
	have_compress=true
	poll_timeout=2000
	interfaces="0.0.0.0:6033;/tmp/proxysql.sock"
	default_schema="information_schema"
	stacksize=1048576
	server_version="5.1.30"
	connect_timeout_server=10000
	monitor_history=60000
	monitor_connect_interval=200000
	monitor_ping_interval=200000
	ping_interval_server=10000
	ping_timeout_server=200
	commands_stats=true
	sessions_sort=true
}


# defines all the MySQL servers
mysql_servers =
(
#	{
#		address = "127.0.0.1" # no default, required . If port is 0 , address is interpred as a Unix Socket Domain
#		port = 3306           # no default, required . If port is 0 , address is interpred as a Unix Socket Domain
#		hostgroup = 0	        # no default, required
#		status = "ONLINE"     # default: ONLINE
#		weight = 1            # default: 1
#		compression = 0       # default: 0
#   max_replication_lag = 10  # default 0 . If greater than 0 and replication lag passes such threshold, the server is shunned
#	},
#	{
#		address = "/var/lib/mysql/mysql.sock"
#		port = 0
#		hostgroup = 0
#	},
#	{
#		address="127.0.0.1"
#		port=21891
#		hostgroup=0
#		max_connections=200
#	},
#	{ address="127.0.0.2" , port=3306 , hostgroup=0, max_connections=5 },
#	{ address="127.0.0.1" , port=21892 , hostgroup=1 },
#	{ address="127.0.0.1" , port=21893 , hostgroup=1 }
#	{ address="127.0.0.2" , port=3306 , hostgroup=1 },
#	{ address="127.0.0.3" , port=3306 , hostgroup=1 },
#	{ address="127.0.0.4" , port=3306 , hostgroup=1 },
#	{ address="/var/lib/mysql/mysql.sock" , port=0 , hostgroup=1 }
)


# defines all the MySQL users
mysql_users:
(
#	{
#		username = "username" # no default , required
#		password = "password" # default: ''
#		default_hostgroup = 0 # default: 0
#		active = 1            # default: 1
#	},
#	{
#		username = "root"
#		password = ""
#		default_hostgroup = 0
#		max_connections=1000
#		default_schema="test"
#		active = 1
#	},
#	{ username = "user1" , password = "password" , default_hostgroup = 0 , active = 0 }
)



#defines MySQL Query Rules
mysql_query_rules:
(
#	{
#		rule_id=1
#		active=1
#		match_pattern="^SELECT .* FOR UPDATE$"
#		destination_hostgroup=0
#		apply=1
#	},
#	{
#		rule_id=2
#		active=1
#		match_pattern="^SELECT"
#		destination_hostgroup=1
#		apply=1
#	}
)


# http://www.hyperrealm.com/libconfig/libconfig_manual.html#Configuration-File-Grammar
#
# Below is the BNF grammar for configuration files. Comments and include directives are not part of the grammar, so they are not included here.
#
# configuration = setting-list | empty
#
# setting-list = setting | setting-list setting
#
# setting = name (":" | "=") value (";" | "," | empty)
#
# value = scalar-value | array | list | group
#
# value-list = value | value-list "," value
#
# scalar-value = boolean | integer | integer64 | hex | hex64 | float
#                | string
#
# scalar-value-list = scalar-value | scalar-value-list "," scalar-value
#
# array = "[" (scalar-value-list | empty) "]"
#
# list = "(" (value-list | empty) ")"
#
# group = "{" (setting-list | empty) "}"
#
```





Cheatsheet

```
LOAD MYSQL USERS TO RUNTIME; SAVE MYSQL USERS TO DISK;
LOAD MYSQL SERVERS TO RUNTIME; SAVE MYSQL SERVERS TO DISK;
LOAD MYSQL QUERY RULES TO RUNTIME; SAVE MYSQL QUERY RULES TO DISK;
LOAD MYSQL VARIABLES TO RUNTIME; SAVE MYSQL VARIABLES TO DISK;

LOAD ADMIN VARIABLES TO RUNTIME; SAVE ADMIN VARIABLES TO DISK;
```
