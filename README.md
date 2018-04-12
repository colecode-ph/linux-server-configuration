# Linux Server Configuration
Configuring an Ubuntu 16 LTS server to host a Flask application via Apache web server.

This is the sixth and final project for the Udacity Full Stack Web Developer Nanodegree. The goal is to use Amazon Lightsail to deploy a functional web application on the public internet. The objective is to learn how to configure a Linux web server in a secure and functional fashion.
 
The application hosted is actually one of the previous Udacity projects, in this case a book catalog CRUD application built with the python Flask application framework, and the PostgreSQL database server.

# IP Address and SSH Port

The IP address of the application is: `52.77.247.143`

The SSH port is: `2200`

# The URL of the Hosted Application

The URL is: `http://52.77.247.143.xip.io/`

# Summary of Software Installed on the Server

## Software installed via `sudo apt-get install`:
First I ran `sudo apt-get update && apt-get upgrade` and `sudo apt-get dist-upgrade` to bring the server up to date. 

Then I installed the following packages:

* apache2
* libapache2-mod-wsgi
* postgresql
* python-pip

## Software installed via `sudo pip install`:
First I ran `pip install --upgrade pip` to get to the latest version. Then I installed the following packages:

* requests
* sqlalchemy
* psycopg2
* oauth2client
* flask
* flask-httpauth
* flask-sqlalchemy
* flask-bootstrap

# Summary of Configuration Changes Made

## Configured the firewall

Configured the firewall to only allow access to ports 22, 2200, 80, and 123. (remove port 22 access once we change the SSH config to only allow access on port 2200)

* `sudo ufw allow 22`
* `sudo ufw allow 2200`
* `sudo ufw allow 123`
* `sudo ufw allow 80`
* `sudo ufw enable`

## Configured proper SSH access

Edited the ssh server configuration file: `sudo nano /etc/ssh/sshd_config`

Added the following:

* `Port 2200` - changes ssh port access to 2200 - remove or comment the reference to port 22 also
* `PermitRootLogon no` - completely disallow root logon - comment or remove any conflicting lines as well
* `ClientAliveInterval 50` - add client interval to prevent Lightsail automatic disconnection of session

Once I saved and restarted the sshd service, I then blocked access to port 22: `sudo ufw delete allow 22`

View the current firewall ruleset:
```
ubuntu@ip-172-26-0-195:~$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
2200                       ALLOW       Anywhere                  
80                         ALLOW       Anywhere                  
123                        ALLOW       Anywhere                  
2200 (v6)                  ALLOW       Anywhere (v6)             
80 (v6)                    ALLOW       Anywhere (v6)             
123 (v6)                   ALLOW       Anywhere (v6)
```
### Configured the PostgreSQL server




## Configured Apache Web Server
Since there's only going to be one application running on this server, I used the `000-default.conf` file for configuration.

I added the appropriate information for document root and directory:
```
DocumentRoot /home/ubuntu/bookcatalog
        <Directory />
            Options FollowSymLinks
            AllowOverride None
       </Directory>
       <Directory /home/ubuntu/bookcatalog/>
            Options Indexes FollowSymLinks MultiViews
            AllowOverride None
            Require all granted
        </Directory>
```
I also added a line for the WSGI file: ` WSGIScriptAlias / /home/ubuntu/bookcatalog/myapp.wsgi`

I also changed the ownership of the application files and directories to www-data:www-data using the chown command.

The myapp.wsgi file was also populated with the appropriate lines to make it work:
```
import sys
sys.path.insert(0,"/home/ubuntu/bookcatalog/")

from catalog import app as application
application.secret_key = '####'
```

