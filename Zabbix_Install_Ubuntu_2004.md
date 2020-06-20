# Zabbix 5.0 Installation for Ubuntu 20.04

## Installing Zabbix Server

### Quick Note About Code Examples

You will see `# ` used to signify a command line in Bash throughout this document. Do not confuse it with comments you will see in the configuration files `#comment`. Be sure to uncomment configurations and save the documents or you may have trouble following the guide.

### Begin Installation

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

You will want to make sure you enter the correct **root** account password to connect to mysql server. Once you connect to mysql server, you then create the initial database and assign a mysql username / password to connect to the database. 

Since this demo has an account that is already root, we can just run the commands below:

```Bash
# sudo mysql

mysql> create database zabbix character set utf8 collate utf8_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> quit;
```

[*There's a really good tutorial on this process from a MySQL perspective here.*](https://linuxize.com/post/how-to-create-mysql-user-accounts-and-grant-privileges/) That should explain things a little better. Be sure to make note of what you used for the mysql user password above. For this demo, let's leave it at '**password**'.

At this point, you have an empty database. There are no tables or structure. The next step is the import the schema using a script provided by Zabbix:. This is an important distinction to know about. The `-uzabbix -p zabbix` is not the information` for your zabbix installation, but that of the database from which you are copying the schema from that is provided by Zabbix. 

```Bash
# zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix
```

This will take a minute or two to execute. When you are returned to the command prompt, you can continue. The username and password shown above is for the schema being imported, it is not the same account you will be using going forward. 

## Configuring Zabbix Server

Using the built in text editor that comes with Ubuntu Server, *nano*, we will make a couple of edits to add the database connection password to the */etc/zabbix/zabbix_server.conf* file:

```Bash
# sudo nano /etc/zabbix/zabbix_server.conf
```

Using the keyboard shortcut 'CTRL+W' to search, type in '**DBPassword**' and pressing **Enter** should put your cursor directly at the configuration variable you need to edit.

Type in the password used by the MySQL account created previously, **password**

Next, you will need to set the timezone. To do this, edit */etc/zabbix/apache.conf* using nano:

```Bash
# sudo nano /etc/zabbix/apache.conf
```

There are two fields to edit for PHP. Be sure to edit both from the defaults to **America/New_York**. Use **CTRL+W** to search for this line: 

```text
# php_value date.timezone Europe/Riga
```

Be sure to uncomment both lines. Your edit should look like this:

```text
php_value date.timezone America/New_York
```

## Starting Zabbix Server

If all of your edits are correct, lets go ahead and restart the Zabbix services and enable them so they will auto start on reboot:

```Bash
# systemctl restart zabbix-server zabbix-agent apache2
# systemctl enable zabbix-server zabbix-agent apache2
```

You should now be able to browse to to the Zabbix Web frontend and finish the initial configuration. 

From your web browser, navigate to `http://[IP_Address]/zabbix/` and follow the on screen instructions. At this point you will see if the edits are correct or the setup wizard will not proceed. When asked for the database password, you will use the user and password created earlier which was **zabbix** / **password**. Keep clicking next and use the defaults for now.

The default login is **Administrator** / **zabbix**.
*Usernames case sensitive!*

## Enable VMware Monitoring Capabilities

While Zabbix supports VMware monitoring out of the box, that doesn't mean it's enabled. You have to make a couple of more edits to the server configuration.

```Bash
sudo nano /etc/zabbix/zabbix_server.conf
```

Using **CTRL+W**, search for and edit the following line:

```text
# StartVMwareCollectors=0
```

to this:

```text
# StartVMwareCollectors=5
```

Using **CTRL+X** and choosing **Y** to save the file, you need to restart the Zabbix Server Daemon to apply the configuration changes:

```Bash
# sudo systemctl restart zabbix-server
```

**Note** 

There are some additional settings that can be adjusted, but by default, it will take up to an hour for metrics to be collected. For the demo, you will see I changed that to 10 minutes. Defaults are fine for most applications.
