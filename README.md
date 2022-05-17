# IdeaProjects
Web Development using java

## setting up tomcat server
	- make a user specially dedicated to tomcat with some permissions
		- ``` sudo useradd -m -d /opt/tomcat -U -s /bin/false tomcat ```
	- now install the default-jdk (recommended) or anyjdk
		- ``` sudo apt-get install default-jdk ```
	- download the tar.gz core source files from tomcat official site
		- ``` cd /tmp ``` (or any preffered directory)
		- ``` wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.0.20/bin/apache-tomcat-10.0.20.tar.gz ``` (downloaded files in temp directory)
		- `` sudo tar xzvf apache-tomcat-10*tar.gz -C /opt/tomcat --strip-components=1 ``` (extracting the file in /opt/tomcat directory)
		- ``` sudo chown -R tomcat:tomcat /opt/tomcat/ ``` and ``` sudo chmod -R u+x /opt/tomcat/bin ``` (grant tomcat user "created above"  ownership over the installation folder).
		- ``` sudo nano /opt/tomcat/conf/tomcat-users.xml ``` (configure admin and manager roles to get access to the tomcat dashboard).
		- ```
 <role rolename="manager-gui" />
<user username="manager" password="manager_password" roles="manager-gui" />

<role rolename="admin-gui" />
<user username="admin" password="admin_password" roles="manager-gui,admin-gui" /> ``` (this help us to login and monitor the tomcat server application and other stuff).
		- ```sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml ``` (comment the valve defination to connect and facilate the conection over any host name)
		- ``` sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml ``` (do same for this to)
		- ``` sudo nano /etc/systemd/system/tomcat.service ``` (make a file for configuring the start of the server)
		- contents will be ```
[Unit]
Description=Tomcat
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
``` (change the java_home part accordingly)
		- now just reload the daemon of systemctl ``` sudo systemctl daemon-reload ```
		- ``` systemctl start tomcat ``` (to start manually)
