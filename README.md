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


      4. Jenkins setup is complete and it can be accessed with the URL that is configured for it.







