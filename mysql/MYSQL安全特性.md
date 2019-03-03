

# MYSQL安全特性

## 1、准入安全

- 修改默认网络端口

  `/etc/mysql/mariadb.conf.d/server.cnf`

- 添加登陆账号并限制IP

  `Create user 'test'@'localhost' identified by '11111';`

  `update user set password=password('12345678') where user='test';`

- 论登陆方式的重要性

## 2、行为安全

- 新建账号默认权限

  `show grants for test;`  usage就是基本无权限；

- 创建数据库和表

  `desc test;`

  `show create table test;`

- 赋予查询权限以及收回权限

  `grant select,update,delete on pldsec.test to test;`

   `revoke select on pldsec.test from test;`

  注意赋予什么权限回收时也需要写相同的；

- With grant option

  `grant select(id,name) on pldsec.test to test with grant option;`

  `revoke grant option on pldsec.test from test;`

## 3、业务安全

`	mysql_real_escape_string($pw)` '\ '

`magic_quotes_gpc = Off`

`addslashes()` '' \''

对SQL语句进行预编译；

## 4、数据库审计

MYSQL只有企业版本有，也可以用第三方插件macfee插件或是init-connect+binlog方案实现；

- 查看plugin安装目录并安装审计插件

  `show variables like '%plugin%';`

  `INSTALL PLUGIN server_audit SONAME 'server_audit.so';`

- 查看审计全局变量

   `show global variables like '%audit%';`

- 开启审计日志并查看审计状态

  `set global server_audit_logging=1;`

  `show global status like '%audit%';`

- 默认保存在本地的server_audit.log文件中，如果需要SYSlog

  `SET GLOBAL server_audit_output_type=SYSLOG;`

  server_audit_logging=1配置mysql重启后失效，所以需要写入配置文件[mysqld]下，重启后自动开启

  `server_audit_logging=on` 

  `server_audit=FORCE_PLUS_PERMANENT #防止插件被卸载`

## 5、数据库漏洞

- 数据库查询版本

  `select version();`

  [数据库升级方法](https://cloud.tencent.com/info/67f5bec98def09559c1dfd37ce187385.html)

## 6、加密传输

Task1:mysql网络加密传输

## 7、其他安全项

- 数据库脱敏

  `select insert(tel,4,7,'****') as tel from test;`

  `select concat(left(tel,3),'****',right(tel,4)) as tel from test;`

- system

  允许运行系统命令，远程连接无法执行。

- File

  1、file权限；2、secure_file_priv不为NULL

  load data infile方法

  `load data infile '/tmp/1.txt' into table user;`

  Load_file方法

  `create table user(cmd text);`

  `insert into user(cmd) values (load_file('/tmp/1.txt'));`

  `select * from user;`

- 数据库备份

​        `mysql -uroot -p tysql < 1.sql`

`	mysqldump -uroot -p tysql （表名）> 1.sql#导出数据可以加表名`

task:Xtrabbackup 全量备份,增量备份

- 无密码登陆MYSQL ROOT

  关闭数据库

  进入终端输入：cd /usr/local/mysql/bin/

  回车后 登录管理员权限 sudo su

  回车后输入以下命令来禁止mysql验证功能 ./mysqld_safe --skip-grant-tables &

  //绕过mysql的权限限制直接登陆数据库；

  登陆数据库修改配置，重启数据库；

  

  
