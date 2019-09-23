### 修改mysql命令行提示符  
mysql -uroot -p --prompt "mysql_\U >"
prompt `mysql_\U >`
- \U：user_name@host_name

### 获取初始密码

grep "temporary password" /var/log/mysqld.log

### 安全配置向导
mysql_secure_installation

Enter current password for root (enter for none):<–初次运行直接回车

Set root password? [Y/n] <– 是否设置root用户密码, 刚才我已设置密码，我们输n

Remove anonymous users? [Y/n] <– 是否删除匿名用户,生产环境建议删除，所以直接回车或Y

Disallow root login remotely? [Y/n] <–是否禁止root远程登录,根据自己的需求选择Y/n并回车,建议禁止

Remove test database and access to it? [Y/n] <– 是否删除test数据库,直接回车或Y

Reload privilege tables now? [Y/n] <– 是否重新加载权限表，直接回车

### 导入导出数据库

#### 导入数据库

mysql -uroot -p [数据库]<[数据库.sql]

\> use [数据库];  
\> source [数据库路径.sql]

#### 导出数据库

mysqldump -uroot -p [ -A | -B ] [数据库]>[数据库.sql]

- -A，导出全部数据库
- -B，导出整个库包含建库语句

select ... into outfile [导出路径];
