
## 1. Objective

To **automatically deploy a Java web application (WAR file) on Apache Tomcat** using **Jenkins**, without manual intervention, by configuring **Jenkins triggers (CRON)**.

## 2. Tools & Technologies Used

- **Apache Tomcat 9**    
- **Jenkins**    
- **Deploy to Container Jenkins Plugin**    
- **Java WAR file**    
- **Linux (Ubuntu)**    
- **CRON Scheduler**

## 3. Concept of Auto Deployment

**Auto deployment** is the process of deploying application code to a server **automatically** whenever a job is triggered, without manually copying files or restarting services.

In this setup:

- Jenkins acts as the **automation tool**    
- Tomcat Manager acts as the **deployment endpoint**    
- CRON triggers Jenkins at fixed intervals    
- Deployment happens automatically

## 4. Architecture Diagram
![[Pasted image 20251222013650.png]]
## 5. Apache Tomcat Setup

### 5.1 Install Tomcat

download a sample war file from this [link](https://tomcat.apache.org/download-90.cgi) 

```
sudo su
cd /opt
wget https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample

tar -xvzf apache-tomcat-9.0.113.tar.gz
```
	
### 5.2 Change Tomcat Port 

Edit:

```
nano conf/server.xml
```

Change:

```
<Connector port="8080">
```

to:

```
<Connector port="9090">
```

![[Pasted image 20251222010944.png]]

### 5.3 Start Tomcat

```
cd bin 
./startup.sh
```

Access:

```
http://<IP>:9090
```

![[Pasted image 20251222011112.png]]

## 6. Tomcat Manager Configuration

### 6.1 Configure Users and Roles

Edit:
```
nano conf/tomcat-users.xml
```

Add:

```
<role rolename="manager-gui"/>
   <role rolename="manager-script"/>
   <role rolename="manager-jmx"/>
   <role rolename="manager-status"/>
   <user username="admin" password="admin" roles="manager-gui,manager-script,manager-jmx,manager-status"/>
   <user username="deployer" password="deployer" roles="manager-script"/>

```

![[Pasted image 20251222011215.png]]
### 6.2 Enable Remote Deployment

Edit:

```
nano webapps/manager/META-INF/context.xml
```

Use:

```
<Context privileged="true">      
<CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor" />  
<!-- Allow remote Jenkins access -->  
<!--     <Valve className="org.apache.catalina.valves.RemoteCIDRValve"            allow="127.0.0.0/8,::1/128" />     --> 

 </Context>
```

![[Pasted image 20251222011250.png]]

Restart Tomcat:

```
./shutdown.sh 
./startup.sh
```


## 7. Jenkins Setup

### 7.1 Install Jenkins

(Pre-installed or installed via package manager)

Verify:

```
systemctl status jenkins
```

### 7.2 Install Required Plugin

- Go to **Manage Jenkins → Plugins**
    
- Install **Deploy to Container Plugin**
    

---

## 8. Jenkins Job Configuration

### 8.1 Create Job

- New Item → **Freestyle Project**
    
- Job name: `Tomcat-Auto-Deployment`
### 8.2 Build Step

Add **Execute shell**:

```
	pwd
```

run the job now the workspace will be created , in that place the sample app war file in this workspace of your job

WAR file should exist in workspace:

```
/var/lib/jenkins/workspace/Tomcat/sample.war
```

### 8.3 Post Build Action (Deployment)

Add **Deploy war/ear to a container**

|Field|Value|
|---|---|
|WAR/EAR files|`**/*.war`|
|Context Path|`sample`|
|Container|Tomcat 9.x|
|Tomcat URL|`http://<IP>:9090`|
|Credentials|deployer / deployer|
![[Pasted image 20251222011607.png]]
## 9. Auto Deployment Using CRON

### 9.1 Enable CRON Trigger
- Go to **Job → Configure**    
- Check **Build periodically**   
### 9.2 CRON Expression

Example (every 5 minutes):
```
 * * * * *
```

This means Jenkins automatically:
- Runs the job    
- Deploys the WAR file    
- Refreshes the application on Tomcat
![[Pasted image 20251222011716.png]]
## 10. How Auto Deployment Works (Explanation)

When the CRON trigger runs:

1. Jenkins job starts automatically    
2. Jenkins connects to Tomcat Manager    
3. Old application is undeployed    
4. New WAR file is deployed    
5. Application is live without manual steps   

This completes **auto deployment**.


## 11. Verification
### Jenkins:
- Build shows **SUCCESS**   
![[Pasted image 20251222012037.png]]
### Tomcat:

```
http://<IP>:9090/sample
```

Application loads successfully.

![[Pasted image 20251222011843.png]]
## Conclusion

This project demonstrates **auto deployment of applications using Jenkins and Apache Tomcat**. By configuring Jenkins triggers and Tomcat Manager, application deployment is fully automated, enabling continuous delivery and DevOps best practices.