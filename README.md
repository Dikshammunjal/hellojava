## Simple DevOps for CI/CD through Jenkins

1. Provision Jenkins on OCI
2. Provision Tomcat on OCI
3. Install Git and Maven in Jenkins
4. Configure a Job in Jenkins to build the Java Code
5. Configure a Job in Jenkins to deploy the generated war to Tomcat Server and access the web content via browser
6. Automatic Builds in Jenkins


### Provision Jenkins on OCI

1. Create a compute instance in OCI , download the public and private keys for the instance for ssh
2. ssh to instance using keys
3. Java needs to be installed and configured on the server on which you want to configure Jenkins. OpenJDK is preferred with Jenkins, but you can also use any other version of Java.

    yum install java-11-openjdk-devel

    1. If multiple Java versions are installed on your server, you can specify the default Java version using this command:

       update-alternatives --config java
       
    2. Install the wget tool in your operating system to fetch the Jenkins repository:

       yum install wget
    
    3. To install Jenkins on to your operating system, follow the latest documentation provided by Jenkins. At the time of writing, you first need to configure  yum by adding the Jenkins repository and then import the repository GPG key:
      
      `wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
      
      rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key`
  
    4. You can check the presence of the repo using this command:

      yum repolist
   
    5. The following links are for the LTS version for Jenkins. You can also use the latest build. When the repository is updated, you need to install Jenkins and start the service. Using the systemctl start command starts the Jenkins service and enabling the service will start it on bootup.

    6. To check if the Jenkins service is running, use the command:

      systemctl status jenkins

    7. You also need to add Jenkins service to run with firewall and add its exception so that it is available to access from the outside world. Finally, we need to reload the firewall service for the changes to take effect.

      firewall-cmd --add-port=8080/tcp --permanent
      firewall-cmd --reload
  
    8. To check the firewall status and accessible ports, use the firewall-cmd command:

      firewall-cmd --list-all
      Now, the Jenkins server will be running on port 8080 for our server.

4. Configuring Jenkins

      1. You can configure the Jenkins service on port 8080 of your system, but Jenkins is temporarily locked with a password present in the    /var/lib/jenkins/secrets/initialAdminPassword file. You can access Jenkins by providing the password after reading the file.


      Remember to open the file with root user permissions as it is not accessible otherwise.

      2. Install the suggested plugins for Jenkins. They are compatible with most versions, but if you want to do something specific, you can also select     and work with the plugins you wish.

      The plugins will take some time to install depending on the connectivity speed, so be patient.

      3. Create an admin user. Make sure you remember the username and password, as they are the credentials for accessing the Jenkins WebUI.
      Specify if you wish to change the port for your Jenkins. It is preferred to use Jenkins on 8080 port.


      4. Jenkins setup is complete and it can be accessed with the URL that is configured for it.http://155.248.243.6:8080/login?from=%2F 

      
      Reference:https://www.redhat.com/sysadmin/install-jenkins-rhel8


### Provision Tomcat on OCI

1. Create a compute instance in OCI , download the public and private keys for the instance for ssh
2. ssh to instance using keys
3. sudo su 
4. yum update
5. yum install java-1.8*
6. cd /opt/
7. Go to https://tomcat.apache.org/download-90.cgi and copy link of tar gz 
8. wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.60/bin/apache-tomcat-9.0.60.tar.gz
9. tar xvzf apache-tomcat-9.0.60.tar.gz

!<img width="563" alt="image" src="https://user-images.githubusercontent.com/57708209/159464982-c32b8a90-fa22-4214-9ff7-ae4619b92a7b.png">


10. cd apache-tomcat-9.0.60/
11. ls
12. cd bin/
13. chmod +x shutdown.sh
14. chmod +x startup.sh
15. ./startup.sh
16. It should give message as Tomcat Started
17. firewall-cmd --zone=public --add-port=8080/tcp --permanent
18. firewall-cmd --reload
19. copy the ip address of tomcat instance and try to launch on default 8080 port. Make sure the security lists are update to allow traffic
20. Click server status -it might give access denied error
21. Need to update users list as given in link- https://github.com/ValaxyTech/DevOpsDemos/blob/master/Tomcat/tomcat_installation.MD
22. cd /opt/apache-tomcat-9.0.60/webapps/manager/META-INF
23. vim context.xml
24. !<img width="569" alt="image" src="https://user-images.githubusercontent.com/57708209/159466912-ac20190c-26de-4272-8ed2-5431f32130b0.png">

25. comment the content around classname tag
26. chmod +x context.xml
27. cd /opt/apache-tomcat-9.0.60/conf
28. ls
29. chmod +x tomcat-users.xml
30. Copy and paste role and users from above link to this file
31. <img width="581" alt="image" src="https://user-images.githubusercontent.com/57708209/159467685-dc4b390e-654a-474a-9e8c-63f7f07a41ff.png">
32. cd /opt/apache-tomcat-9.0.60/bin
33. ./shutdown.sh
34. ./startup.sh
35. Should see message as "Tomcat Started"
36. You can username and passwrod as admin/admin as pasted in tomcat-users.xml file
37. You should be able to click on server status button and Manager App buttons
38. Reference Link-https://www.youtube.com/watch?v=elwjlkBiJgE

39. If you wish to change the port to 80
40. cd /opt/apache-tomcat-9.0.60/conf
41. vim server.xml 
42. update connector port to 80
43. <img width="568" alt="image" src="https://user-images.githubusercontent.com/57708209/159468463-ace3e7eb-b534-430f-bc21-2923ab054c83.png">
44. Save the file , OpenFirewall on port 80 
45. shutdown and startup tomcat
46. http://144.24.103.222:80


### Install Git and Maven in Jenkins

1. ssh to Jenkins Machine
2. sudo yum install git
3. git --version
4. Output like git version 2.27.0 should come
5. which git
6. export PATH=$PATH:/place/with/the/file
7. Where /place/with/the/file is the path to the Git installation.
8. Once you performed this change, you can log in again in your system and check it running the git --version command and the which git command.
9. Jenkins will need to be restarted for the PATH environment variable change to be reflected in Jenkins.
10. Login into the Jenkins instance as an administrator.
11. Go to Manage Jenkins -> Global Tool Configuration -> Git -> Git installations
12. Type git in the Path to Git executable of each Git installation where you want to use the Git installation binary included in the $PATH environment variable.
13. <img width="963" alt="image" src="https://user-images.githubusercontent.com/57708209/159470342-00f56b7a-3dd0-475a-bc83-2819fdb024f8.png">

14. Reference Link-https://support.cloudbees.com/hc/en-us/articles/360048202171-Using-PATH-in-Git-Configuration
15. Go to https://maven.apache.org/download.cgi
16. copy link for apache-maven-3.8.5-bin.tar.gz	
17. sudo su -
18. cd /opt
19. wget 
20. tar xvzf apache-maven-3.8.5-bin.tar.gz	
21. cd /opt/apache-maven-3.8.5
22. <img width="1034" alt="image" src="https://user-images.githubusercontent.com/57708209/159471546-e833372f-b5ad-4212-8d0e-8e814e30ce5b.png">

23. Paste this in under Global configurations MAVEN_HOME
24. if we run mvn in current path it will error saying command not found. if we go inside bin and run ./mvn it will run. that means not available for all users
25. cd bin
26. ln -s /opt/apache-maven-3.8.5/bin/mvn /bin
27. su - jenkins
28. mvn --version
29. It should work now.
30. Reference Link- https://www.youtube.com/watch?v=BxBHrm_lf4Q


### Configure a Job in Jenkins to build the Java Code
### Configure a Job in Jenkins to deploy the generated war to Tomcat Server and access the web content via browser
### Automatic Builds in Jenkins

Install "Deploy to Container" Plugin. Once its installed you should be able to see the option to deploy war/ear files to conatiner under post-build actions

<img width="1253" alt="image" src="https://user-images.githubusercontent.com/57708209/159479864-aa97a460-42f7-4b2a-a899-699b63ceb63a.png">

<img width="906" alt="image" src="https://user-images.githubusercontent.com/57708209/159479941-daab8ede-dc56-45ea-b78d-3dde80b7c159.png">

<img width="1002" alt="image" src="https://user-images.githubusercontent.com/57708209/159480020-76e366f8-7078-474b-a176-6f8e9395f10d.png">


<img width="1014" alt="image" src="https://user-images.githubusercontent.com/57708209/159480111-5203e2cc-095c-459a-8397-d55adcd61cbe.png">


<img width="1305" alt="image" src="https://user-images.githubusercontent.com/57708209/159480620-d0d1fd95-18c2-419c-a17f-2ce9f8e89ae5.png">

To verify ssh to tomcat machine and you will find webapp.war and webapp folder getting created

<img width="594" alt="image" src="https://user-images.githubusercontent.com/57708209/159481677-9ce41a21-3ae9-4e8e-bbc7-1201b5c86c36.png">


http://144.24.103.222/webapp/

<img width="1121" alt="image" src="https://user-images.githubusercontent.com/57708209/159481833-82dad167-9417-4dbe-8208-d75fa6fd81be.png">




















