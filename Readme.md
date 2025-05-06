# Automating CI CD with Jenkins on Google Cloud

This is a sample demo showcasing how to perform CI/CD integration using Jenkins. It covers the comprehensive process of setting up DevOps tools on Google Cloud, integrating Jenkins with Tomcat for automated deployments, and performing Git workflow operations.

I'm excited to share these steps with the community to help others understand how to build a complete CI/CD pipeline in the cloud.

---

##  Infrastructure Overview

To accomplish this task, I created **4 Virtual Machines** (VMs) in Google Cloud:

| VM        | Purpose                                                        |
|-----------|----------------------------------------------------------------|
| platform  | Acts like a developer machine for handling production code     |
| jenkins   | Hosts Jenkins for CI/CD automation                             |
| tomcat-1  | Tomcat server for internal deployment and testing              |
| tomcat-2  | Another Tomcat server for internal sharing and redundancy      |

<img width="1507" alt="Screenshot 2025-05-05 at 17 44 23" src="https://github.com/user-attachments/assets/7ad54d7d-e807-44ee-a475-f6f8d27e9397" />

---

## CI/CD Pipeline Steps

###   Step 1: Clone GitHub Repository to Platform VM
I have cloned my project repository to the platform VM, simulating a developer pushing code changes.

---

###   Step 2: Configure Jenkins VM
- Installed Jenkins
- Set up Java, Git, and required dependencies
- Installed necessary Jenkins plugins (e.g., Git, Deploy to Container, etc.)  

This guide outlines the steps i followed  to install Java 17, Apache Maven, on Ubuntu system. The same steps should be followed for both Jenkins and Tomcat machines.

```
sudo apt update
sudo apt install fontconfig openjdk-17-jre
sudo apt install vim wget git -y
```

```
# Download and install Apache Maven
wget https://dlcdn.apache.org/maven/maven-3/3.9.8/binaries/apache-maven-3.9.8-bin.tar.gz
tar xvf apache-maven-3.9.8-bin.tar.gz
sudo mv apache-maven-3.9.8 /usr/local/apache-maven
```

```
# Configure Environment Variables  
vim ~/.profile
```

```
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64/
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
export M2_HOME=/usr/local/apache-maven
export M2=$M2_HOME/bin
export PATH=$M2:$PATH
```

```
sudo vim /etc/environment
```

```
# add:
:/usr/lib/jvm/java-17-openjdk-amd64/bin:/usr/local/apache-maven/bin"
```

```
`source ~/.profile
```


```
# Verify
mvn -version
java --version
```

<img width="699" alt="Screenshot 2025-05-06 at 13 01 52" src="https://github.com/user-attachments/assets/399282e1-d20d-4460-8201-82c4a938a555" />

###   Step 3: Configure Tomcat VM  

For creating all step above for install mvn and java. then followed below steps/

```
# mkdir /usr/local/tomcat 
sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.90/bin/apache-tomcat-9.0.90.tar.gz 
sudo tar xvf apache-tomcat-9.0.90.tar.gz --strip-components=1 -C /usr/local/tomcat 
sudo ln -s /usr/local/tomcat/apache-tomcat-9.0.90 /usr/local/tomcat/tomcat 
sudo useradd -r tomcat 
sudo chown -R tomcat:tomcat /usr/local/tomcat 
```

```
# vim /etc/systemd/system/tomcat.service  

[Unit]  
Description=Apache Tomcat Web Application Container  
After=network.target

[Service]
Type=forking
User=tomcat
Group=tomcat
Environment="JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom -Djava.awt.headless=true"
Environment="CATALINA_BASE=/usr/local/tomcat"
Environment="CATALINA_HOME=/usr/local/tomcat"
Environment="CATALINA_PID=/usr/local/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"
ExecStart=/usr/local/tomcat/bin/startup.sh
ExecStop=/usr/local/tomcat/bin/shutdown.sh

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl daemon-reload
sudo systemctl enable tomcat
sudo systemctl start tomcat
sudo systemctl status tomcat
```

```
# sudo tee /usr/local/tomcat/conf/tomcat-users.xml > /dev/null 

<tomcat-users>
    <role rolename="tomcat"/>
    <role rolename="manager-gui"/>
    <user username="tomcat" password="s3cret" roles="tomcat,manager-gui,manager-script"/>
</tomcat-users>
```

```
sudo sed -i 's/<Valve className="org.apache.catalina.valves.RemoteAddrValve"/  

<!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"/' /usr/local/tomcat/webapps/manager/META-INF/context.xml
sudo sed -i 's/\/> -->/\/> -->/' /usr/local/tomcat/webapps/manager/META-INF/context.xml
```

```
sudo systemctl daemon-reload
sudo systemctl stop tomcat
sudo systemctl start tomcat
sudo systemctl status tomcat
```

###  Configure Jenkins VM  
- Installed Jenkins
- Set up Java, Git, and required dependencies
- Installed necessary Jenkins plugins (e.g., Git, Deploy to Container, etc.)

#  Configure Tomcat VMs  
- Installed and configured Apache Tomcat
- Enabled `manager-script` role for remote deployments
- Verified accessibility of `/manager/text` endpoint

---

##  What I Learned

- **Jenkins Setup on GCP**: Learned how to install and configure Jenkins in a VM environment with necessary tools and variables.
- **CI/CD Pipeline Creation**: Built pipelines to automate deployment from Jenkins to Tomcat.
- **Git Mastery**: Practiced advanced Git commands for branching, merging, and managing development workflows.
- **Understanding SDLC**: Gained insights into managing and automating the software development lifecycle, from code push to production deployment.

---

"Readme.md" [readonly] 59L, 2485B                                                                                                                                           1,1           Top
