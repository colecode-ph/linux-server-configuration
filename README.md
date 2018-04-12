# Linux Server Configuration
Configuring an Ubuntu 16 LTS server to host a Flask application via Apache web server.

This is the sixth and final project for the Udacity Full Stack Web Developer Nanodegree. The
 goal is to use Amazon Lightsail to deploy a functional web application on the public internet. The objective is to learn how to configure a Linux web server in a secure and functional fashion.
 
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

## Configure the firewall

Configure the firewall to only allow access to ports 22, 2200, 80, and 123. (We will then remove port 22 access once we change the SSH config to only allow access on port 2200)

* `sudo ufw allow 22`
* `sudo ufw allow 2200`
* `sudo ufw allow 123`
* `sudo ufw allow 80`
* `sudo ufw enable`

## Configure SSH access to port 22 and disable root login

Edit the ssh server configuration file: `sudo nano /etc/ssh/sshd_config`

Make sure the following lines are present:

* `Port 2200` - remove or comment the reference to port 22 also
* `PermitRootLogon no`
* `ClientAliveInterval 50` - add client interval to prevent Lightsail automatic disconnection of session
* `PermitRootLogin no` - prohibit root login - comment or remove any conflicting lines as well

`



