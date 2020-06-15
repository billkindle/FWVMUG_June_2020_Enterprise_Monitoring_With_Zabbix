# Zabbix 5.0 Installation for Ubuntu 20.04

## Installing Zabbix Server
First and foremost, you need to add the Zabbix repository to your Ubuntu VM guest:

``` Bash
# wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.0-1+focal_all.deb
# dpkg -i zabbix-release_5.0-1+focal_all.deb
# apt update
```

Once complete, you need to begin installing the three core Zabbix components:

1. Zabbix Server (which contains MySQL)
2. Zabbix Web Front End (apache2 httpd)
3. Zabbix Agent

```Bash
# apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent
```

With these components installed, you now have to configure the database in order to import the schema for Zabbix. Here you may run into a problem.

You will want to make sure you enter the correct **root** account password to connect to mysql server. Once you connect to mysql server, you then create the initial database and assign a mysql username / password to connect to the database. Run the commands below:

```Bash
# mysql -uroot -p

mysql> create database zabbix character set utf8 collate utf8_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> quit;
```

[*There's a really good tutorial on this process from a MySQL perspective here.*](https://linuxize.com/post/how-to-create-mysql-user-accounts-and-grant-privileges/) That should explain things a little better. Be sure to make note of what you used for the mysql user password above. For this demo, let's leave it at '**password**'.

At this point, you have an empty database. There are no tables or structure. The next step is the import the schema using a script provided by Zabbix:

```Bash
# zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p password
```

This will take a minute or two to execute. When you are returned to the command prompt, you can continue.

## Configuring Zabbix Server

Using the built in text editor that comes with Ubuntu Server, *nano*, we will make a couple of edits to add the database connection password to the */etc/zabbix/zabbix_server.conf* file:

```Bash
# sudo nano /etc/zabbix/zabbix_server.conf
```

Using the keyboard shortcut 'CTRL+W' to search, type in '**DBPassword**' and pressing **Enter** should put your cursor directly at the configuration variable you need to edit.

Type in the password used by the MySQL account created previously.