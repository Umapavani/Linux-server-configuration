Udacity-Linux-Server-Configuration
About
This is the Udacity project 6 about the Configuring the Linux the server.

Server Details
Server IP Address 13.232.29.129

Hosted site Url http://13.232.29.129.xip.io/

How to connect as grader:
save private key provided in your local machine and run the following command

ssh -i path/to/privatekey -p 2200 grader@13.232.29.129
  
Configuring Linux Server
Updating all packages
sudo apt-get update
sudo apt-get upgrade
Creating grader User:
sudo adduser grader
This will add new user

sudo nano /etc/sudoers
Below the Root user append the following line

grader  ALL=(ALL:ALL) ALL
This will grant sudo permission to grader

Creating a ssh key pair for grader
On your local machine in terminal/command prompt

ssh-keygen
This will generate public and private ssh keys which is saved to .ssh folder
id_rsa:
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEA6BPGMkYw45aNAe1S5jr+s00in/+hPtzJC4N7FCPTxZuLz7b8
N6QVk+9sc4EwP4YA7SKJVN3wRJ0MJrMlr6CxJPcisduAi7lfO+b04kR46Jj0Thz3
v+ykBWlKRzzcTPcd3g+mavK257ao2LWpBZ8IDYRsdjRe48V06NT/BtjgUG9qsT5U
70/3AUiJGV4Rj7c+NAIzDwYlY+uM7cAUZULmXU+jHGGHVIGM50W4Q0fn0j50I8sx
shMtKpO6fn0QEFv1SS8M31lgUfAk9G2H+ZEOytJIe9t3IDMYC4qz3rVtAGENugoP
y+hsZo2/mMQq2IPf07tJghfOwo+8zkfzGt2/+QIDAQABAoIBAD1QtCWmO9Z1eU3Q
CP4BCjgPIh3JqS11obxkAYmhqZrk7Lx1aQ++T2Elea7OrBOscOQ2IriEZq2KHKLA
5C0RtJvCm60IBF9mG441B/AcDSHO/4T/zEkt2WwAlHpbzwyaoY6A7gQFmmN/8/5F
iKGOkc8YdJuKXKOrEKdPVw3VEPbO6EHl1Ouj1eJSJ3UYEu1HuwxvwNEZ3MdzQjMA
Yrn4MKbFjxSAF9d6u1FIIOWkvya9A7i2tEwqpl/x4HfG0fnblV9G2sXwAvLyZ5Qd
6O7h5CC/3Pc0cd8ABDw/bmGhCepyLSnBtp7GvyWe0GXtajt639YKQoFIR/nnTuIQ
o5M3bF0CgYEA9LKCzFiysTwL0eBH9Dh3SUcPam3mVYG0tPfIFS1vhRJmCK74VsQZ
xW5WOI0w1D+9SAskvkTWq/SQp6WQ6vh+U9dKLj0LEd3+lqlQlGHY9bVgpg0SUgdo
q1Ajos0YRqcYPy6M+H0GVl53XZC6HUnqgHIrZ6gPYVi45M2FtX2MGQcCgYEA8swI
rwWjNVPOnbcmK7ESrjyVJbtbfEzXlwHNgl6QNAWc22+B/mAyv7XMWkO8BPy7PYvk
VxYFbE+o9/GrBW3FuFlGG+fT2FZTAooD9uDCTKTXq0my/v1aBtSwjY53Q04e1p5P
xRUN2bQy27q/KkzkwYY6IwAmxuODtitvWOUOHv8CgYALwPdfcXND6Cp5LljGdl/k
eTFYX2cM/Gn9t1k5CzGsJrIYAK+VG15RiXdCyCVsTJN+/moJaa0WHW1FYEKRxtXq
mRDLH8uEVDGCcyeXuQt+4fN+hmo23nw8nmDX1Roxap1Ti5zh4r+HLop1SBpohcBp
4xIKrJvwhR58fxLJIPq1yQKBgFkPzv+kqcGeBGSGElJkmd2gC3XTzDNEXLDf0GrK
FUt/45H6zUxqLTN1lIhn4EhUDLr+3bu0MDkS34BT/c/3/FcrKDSETYlF4R1FUz70
I1HKBfKnDinK9YMb8cd7QvRFa7p+R/SbTIFXQHCpiAYUPrVeb2T3YUIDowTn2ehZ
3VwxAoGAcR+BojHBUx/0vPcfkWVJS6XQnYXpqDeWVOxnxWtq8CAbzaJUjTZILdFT
clBBgqvSUZkzZCvmN5qa1+MNclhWeYUWo8Ns6zIfJF55P0Dy6VV8zvj2rjapSUsY
NEJMAB/2gNGVyBlMCXueVG8mGangiZcE9YO361Aw/X7m/sQ2fbQ=
-----END RSA PRIVATE KEY-----


Then in your virtual machine change to newly created user

su - grader
Create a new directory .ssh and new file authorized_keys in that directory

mkdir .ssh
sudo nano .ssh/authorized_keys
Copy the public key with .pub extension to authorized_keys and save the file

chmod 700 .ssh
chmod 644 .ssh/authorized_keys
700 will give read write and execute permission to user.
644 prevent other user from writting in to file. Then restart ssh server
service ssh restart
Now from your log in to grader with private key generated

ssh -i .ssh/id_rsa grader@ipaddress 
Changing the ssh port to 2200
sudo nano /etc/ssh/sshd_config
Change port 22 to port 2200

Restart the ssh server

service ssh restart
Note: Before Logging using ssh add custom TCP port 2200 under lightsaail firewall in networking tab in lightsail instance console

Now Login using command like this

ssh -i .ssh/id_rsa -p 2200 grader@ipaddress
Disabling ssh login as root
sudo nano /etc/ssh/sshd_config

make change PermitRootLogin no

Configurating Ufw firewall
sudo ufw allow 2200/tcp
sudo ufw allow 80/tcp
sudo ufw allow 123/udp
sudo ufw enable
This will allow all required ports and enables the ufw

After that

sudo ufw status
It will display all allowed ports

Changing time Zone
sudo dpkg-reconfigure tzdata

select none from list and then select utc.

Installing Apache2
In terminal

sudo apt-get install apache2

Now mod_wsgi

sudo apt-get install python-setuptools libapache2-mod-wsgi

Enable mod_wsgi

sudo a2enmod wsgi

Setting up your flask application to work with apache2
Creating a flask app

In /var/www directory create a new folder sudo mkdir FlaskApp

Install git

sudo apt-get install git

move to the FlaskApp cd FlaskApp

In that direcory clone your github repository

sudo git clone https://github.com/Umapavani/catalog.git

Rename your repository to FlaskApp

Then rename your project file to __init__.py

Error : While accesssing the client_secrets.json file

PROJECT_ROOT = os.path.realpath(os.path.dirname(__file__))
json_url = os.path.join(PROJECT_ROOT, 'client_secrets.json')
CLIENT_ID = json.load(open(json_url))['web']['client_id']
Use json_url instead client_secrets.json in script

Reffered from stack overflow

Install and configuring postgresql for project
Install Postgres sudo apt-get install postgresql

login to postgres sudo su - postgres

postgres shell psql

create user CREATE USER catalog WITH PASSWORD 'password';

permit user to createdb ALTER USER catalog CREATEDB;

Create a db name catalog with user catalog CREATE DATABASE catalog WITH OWNER catalog;

connect to db \c catalog

revoke all permission to public REVOKE ALL ON SCHEMA public FROM public;

Give schema permission to user catalog GRANT ALL ON SCHEMA public TO catalog;

exit from db and postgres \q and exit

Change the database connection in both db_setup.py and __init__.py as engine = create_engine('postgresql://catalog:password@localhost/catalog')

Now you are ready with your applicatiom

Configure and Enable a New Virtual Host
sudo nano /etc/apache2/sites-available/FlaskApp.conf

In this add the following code

<VirtualHost *:80>
 	ServerName 13.232.29.129
 	ServerAdmin admin@13.232.29.129
 	WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
 	<Directory /var/www/FlaskApp/FlaskApp/>
 		Order allow,deny
 		Allow from all
 	</Directory>
 	Alias /static /var/www/FlaskApp/FlaskApp/static
 	<Directory /var/www/FlaskApp/FlaskApp/static/>
 		Order allow,deny
 		Allow from all
 	</Directory>
 	ErrorLog ${APACHE_LOG_DIR}/error.log
 	LogLevel warn
 	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
Enable the virtual host sudo a2ensite FlaskApp

Disabling the default apache2 page sudo a2dissite 000-default.conf

Create the .wsgi File
```
cd /var/www/FlaskApp
sudo nano flaskapp.wsgi 
```
Add the following code

#!/usr/bin/python
 import sys
 import logging
 logging.basicConfig(stream=sys.stderr)
 sys.path.insert(0,"/var/www/FlaskApp/")

 from FlaskApp import app as application
 application.secret_key = 'Add your secret key'
save and exit

Deploying flask app with apache2 is referred from Digital ocean

Installing require modules
You can either install all modules on your machine or create a virtual environment for the project and install the modules pip install flask sqlalchemy psycopg2 requests oauth2client

Setting up your Google Oauth2
Login to your developer console and select your project and edit OAuth details as following

Javascript origin http://ip.xip.io

redirect URI

http://ip.xip.io\login

http://ip.xip.io\gconnect

http://ip.xip.io\callback

xip.io is a free DNS which will be the same as using IP address

Final Step
Restart your apache2 server

sudo service apache2 restart
