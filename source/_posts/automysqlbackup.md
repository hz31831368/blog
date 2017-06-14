---
title: Linux系统下用AutoMySQLBackup备份MySQL
date: 2017/6/14
---

下载最新版的 AutoMySQLBackup

如果系统基于RPM

```
wget http://downloads.sourceforge.net/project/automysqlbackup/AutoMySQLBackup/AutoMySQLBackup%20VER%203.0/automysqlbackup-v3.0_rc6.tar.gz
```
如果系统基于Debian
```
apt-get update && apt-get upgrade

apt-get install automysqlbackup
```
创建一个目录，并解压Automysqlbuckup到该目录
```
mkdir /opt/automysqlbackup
tar zxvf automysqlbackup-v3.0_rc6.tar.gz -C /opt/automysqlbackup
```
解压完成后运行安装脚本
```
cd /opt/automysqlbackup
./install.sh

### Checking archive files for existence, readability and integrity.

automysqlbackup ... exists and is readable ... md5sum okay :)
automysqlbackup.conf ... exists and is readable ... md5sum okay :)
README ... exists and is readable ... md5sum okay :)
LICENSE ... exists and is readable ... md5sum okay :)

Select the global configuration directory [/etc/automysqlbackup]:
Select directory for the executable [/usr/local/bin]:
### Creating global configuration directory /etc/automysqlbackup:

success
```
安装时需要输入配置文件路径和执行路径，这里可以直接按回车选择默认路径  

接下来需要配置Automysqlbackup，找到上面的配置文件路径，并修改配置文件  
```
vim /etc/automysqlbackup/automysqlbackup.conf
```
常用配置
```
CONFIG_mysql_dump_username='root'
CONFIG_mysql_dump_password='YourPassword'
CONFIG_mysql_dump_host='localhost'
CONFIG_backup_dir='/var/backup/db'
CONFIG_do_monthly="01"
CONFIG_do_weekly="5"
CONFIG_rotation_daily=6
CONFIG_rotation_weekly=35
CONFIG_rotation_monthly=150
CONFIG_mysql_dump_port=3306
CONFIG_mysql_dump_compression='gzip'
CONFIG_db_names=()
```
创建配置里的目录
```
mkdir /var/backup
```
执行automysqlbackup
```
automysqlbackup
```
如果需要定时实行备份，可以使用linux内置命令crontabl添加定时任务  
配置crontab，在开始位置添加环境变量，否则可能会执行失败
```
crontab -e

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

0 1 * * * . /etc/profile; /usr/local/bin/automysqlbackup

```
