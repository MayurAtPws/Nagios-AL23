# Step-by-Step Guide to Install Nagios on Amazon Linux

## 1. Install Required Packages

    sudo su
    yum install httpd php -y
    yum install gcc glibc glibc-common -y
    yum install gd gd-devel -y
    sudo yum install -y wget httpd gcc glibc glibc-common gd gd-devel make net-snmp php

## 2. Create Users and Groups

    adduser -m nagios
    adduser -m apache
    passwd nagios
    groupadd nagioscmd
    usermod -a -G nagioscmd nagios
    usermod -a -G nagioscmd apache

## 3. Prepare Installation Directory

    mkdir ~/downloads
    cd ~/downloads

## 4. Download Nagios Core and Plugins

    wget http://prdownloads.sourceforge.net/sourceforge/nagios/nagios-4.4.6.tar.gz
    wget http://nagios-plugins.org/download/nagios-plugins-2.0.3.tar.gz

## 5. Install Nagios Core

    tar zxvf nagios-4*
    cd nagios-4.4.6
    ./configure --with-command-group=nagioscmd
    make all
    make install
    make install-init
    make install-config
    make install-commandmode
    systemctl enable --now nagios.service

## 6. Configure Nagios Web Interface

    sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin

## 7. Install Nagios Plugins

    cd ~/downloads
    tar zxvf nagios-plugins-2.0.3.tar.gz
    cd nagios-plugins-2.0.3
    ./configure --with-nagios-user=nagios --with-nagios-group=nagios
    make
    make install


## 8. Configure Nagios Service

    chkconfig --add nagios
    chkconfig nagios on

 ## 9. Verify Nagios Configuration

 /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

## 10. Start Nagios and Apache Services
    service nagios start
    service httpd restart


Open a web browser and enter: http://<your_ip_address>/nagios