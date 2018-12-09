# Linux Configuration Project
Configure a web application on a LightSail VM and install the Movie Catalog Flask application created earlier in the nanodegree for Project 4.  

### Connection Info
- URL: `http://54.145.164.69.xip.io`
- IP Address: `54.145.164.69`
- ssh access: `ssh -i udacity-grader.rsa grader@54.145.164.69 -p 2200`  / Passphrase: grader

### Create VM & User access
- Launch Ubuntu VM in AWS LightSail
- Create ssh keys - `ssh-keygen`
- Login to VM and add ssh public RSA key to `authorized_keys`
- Create grader user - `adduser grader`
- Copy ssh public RSA key to grader `authorized_keys`
- Add `grader ALL=(ALL:ALL) ALL` via `visudo`

### Configure Network, ssh, Timezone, and NTP
- Add line '`54.145.164.69 ec2-54-145-164-69.compute-1.amazonaws.com 54.145.164.69.xip.io`' to /etc/hosts file
- Install NTP `sudo apt-get install ntp` and check status `ntpq -p`
- Run 'sudo dpkg-reconfigure tzdata' and configure UTC
- Run `vi /etc/ssh/sshd_config` and change port to 2200
- Change ports for ssh in AWS LightSail
- Execute `sudo service ssh restart` to restart ssh to pick up changes
- Test by logging via ssh using the `-p 2200` option with ssh

### Upgrade all system files
- Run `sudo apt-get update` to get updates
- Run `sudo apt-get upgrade` to install updates

### Install software
- Install Apache2 and mod_wsgi
  `sudo apt-get install apache2,
  sudo apt-get install python-setuptools libapache2-mod-wsgi,
  sudo service apache2 restart`
- Install postgresql - `sudo apt-get install postgresql`
- Install python-pip - `sudo apt-get install python-pip`
- Install Flask, SQLAlchemy & etc.. with pip

### Configure Firewall
- Run the following:
    `sudo ufw allow 2200/tcp,
    sudo ufw allow 80/tcp,
    sudo ufw allow 123/udp,
    sudo ufw enable`
- Run `sudo ufw status` to check status

### Configure postgresql
- Login as postgres and create user `movie_catalog`
- Create database `moviedb`
- Change SQL engine in `database_setup.py` and `moviedb_init.py` files
- Execute `database_setup.py` and `moviedb_init.py` files to build database

### Setup Apache, WSGI and Virtual environment
- Note: Step by step directions are on www.digitalocean.com, URL below.
- Run the following:
  - `sudo apt-get install libapache2-mod-wsgi python-dev`
  - `sudo a2enmod wsgi`
  - `sudo mkdir /var/www/catalog`
  - `git clone https://github.com/sm1507/catalog.git`
  - `mv movie_catalog` to __init__.py
  - `sudo pip install virtualenv`
  - `source venv/bin/activate`
  - `vi /etc/apache2/sites-available/catalog.conf`
  - Update Virtual Host Info
  - `sudo a2ensite catalog`
  - `sudo vi /var/www/catalog/catalog.wsgi`
  - Update catalog.wsgi
  - `sudo service apache2 restart`

### Reboot
- `sudo reboot`

  ## References
  https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
