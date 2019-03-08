# Tomcat
```
sudo yum install epel-release
sudo yum update -y && sudo reboot
```
## Install JAVA Version Java C and Java
### Centos 7
```
sudo yum --showduplicates list java-1.8.*
sudo yum install java-1.8.0-openjdk-1:1.8.0.181-7.b13.el7.x86_64
sudo yum install java-1.8.0-openjdk-devel-1:1.8.0.181-7.b13.el7.x86_64
```
### Centos 6
```
sudo yum --showduplicates list java-1.8.*
yum install java-1.8.0-openjdk-1:1.8.0.181-3.b13.el6_10.x86_64
yum install java-1.8.0-openjdk-devel-1:1.8.0.181-3.b13.el6_10.x86_64
```
## Install Tomcat with Version
Version Tomcat:
```
version List :- https://archive.apache.org/dist/tomcat/tomcat-7/v7.0.90/bin/
cd ~
wget https://archive.apache.org/dist/tomcat/tomcat-7/v7.0.90/bin/apache-tomcat-7.0.90.tar.gz
sudo tar -zxvf apache-tomcat-8.0.33.tar.gz -C /usr/local/tomcat7.90 --strip-components=1
```
Create Link Tomcat:
```
cd /usr/local/
ln -s /usr/local/tomcat7.90 tomcat
```
Permission Tomcat:
```
cd /usr/local/tomcat
sudo chgrp -R tomcat conf
sudo chmod g+rwx conf
sudo chmod g+r conf/*
sudo chown -R tomcat logs/ temp/ webapps/ work/

sudo chgrp -R tomcat bin
sudo chgrp -R tomcat lib
sudo chmod g+rwx bin
sudo chmod g+r bin/*
```

## Systemd Centos 7
Create File For Service Systemctl
```
sudo vi /etc/systemd/system/tomcat.service
```
Add Content in tomcat.service:
```
[Unit]
Description=Apache Tomcat Web Application Container
After=syslog.target network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/bin/kill -15 $MAINPID

User=tomcat
Group=tomcat

[Install]
WantedBy=multi-user.target

```
Save and Exit from VIM Editor:
```
:wq
```
Install Security Package:
### Security Add
Install Package and Enable:
```
sudo yum install haveged
sudo systemctl start haveged.service
sudo systemctl enable haveged.service
```
Start Tomcat and Enable:
```
sudo systemctl start tomcat.service
sudo systemctl enable tomcat.service
```
Start Tomcat add user to Login:
### Tomcat User
```
sudo vi /opt/tomcat/conf/tomcat-users.xml
```
Add Content in file
```
<?xml version='1.0' encoding='utf-8'?>
<tomcat-users>
<user username="yourusername" password="yourpassword" roles="manager-gui,admin-gui"/>
</tomcat-users>
```
Save and Quit:
```
:wq
```
Restart Tomcat with UI:
```
sudo systemctl restart tomcat.service
```
Check Service and Reload Daemon:
```
systemctl list-units
sudo systemctl daemon-reload
```
