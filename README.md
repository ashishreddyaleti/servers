# servers
install tomcat server 8.5.24 on Centos 7
//open Virtual machine with Centos Opertaing system 7 **
// open the terminal with Ipadress in putty **
  
  //  install java latest version with the following code
   
         yum install java 1.7 jdk

    // we have to create a group name  "tomcat"

         groupadd tomcat

    // and we have to add a user tomcat to the group

        useradd -M -s /bin/nologin -g tomcat -d opt/tomcat

    // create directory opt/tomcat

        sudo mkdir opt/tomcat

    // Now get the link of apache tomcat link 8.5.24 of tarfile in home dorectory

        cd ~
        sudo wget http:://apache-tomcat-8.5.24.tar.gz

    //open the tarfile and copy the file into opt/tomcat

        sudo tar zxvf apache-tomcat-8.2.54 -C /opt/tomcat

    //    so the directory structure of the file cd /opt/tomcat/apache-tomcat-8.5.24

    // change the user and group of of opt/tomcat to tomcat

          sudo chown -R tomcat /opt/tomcat
          sudo chown -R tomcat /opt/tomcat

    //Now open apache-tomcat- direcoty 

        sudo cd /opt/tomcat/apache-tomcat/


     // By listing the files of apache-tomcat that consist of conf/webapps/works/logs/temp

         1. change mode of conf with read and execute
            
             sudo chmod g+r conf
             sudo chmod g+x conf
         2. change group and owner ship of webapps/works/logs /temp file to tomcat
          
             sudo chown -R tomcat webapps/works/temp/logs
             sudo chgrp -R tomcat webapps/works/temp/logs

     // Now all the permission are set 
     // create a file tomcat.service for systemd unit file for apache tomcat

        sudo vi etc/systemd/system/tomcat.service

    // copy the below script 
    ********************************************************************************
 
 
 # systemd unit file for tomcat
[Unit]
Description=Apache Tomcat Web Application Container
After=syslog.target network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment=CATALINA_PID=/opt/tomcatapache-tomcat-8.5.24/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat/apache-tomcat-8.5.24
Environment=CATALINA_BASE=/opt/tomcat/apache-tomcat-8.5.24
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/apache-tomcat-8.5.24/bin/startup.sh
ExecStop=/bin/kill -15 $MAINPID

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
~************************************************************************************************

// save and exit file and Now reload Systemd to load the Tomcat unit file:

sudo systemctl demon -reload

// Now start the tomcat

    sudo systemctl start tomcat

        

    
