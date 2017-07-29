. Write a script OR Convey a proven mechanism to retrieve the URL of artifacts uploaded in Nexus / Artifactory via the Jenkins job.

For the above question one of the solution can be using maven and nexus staging maven plugin and nexus jenkins plugin in jenkins.
 As we know maven can be integrated by many tools by providing respective dependencies.
 
 1)install nexus plugin in jenkins.
 2)conFigure POM.xml ,global-settings files as below.
 
Assuming a maven project.

----> POM.xml files should contain.
 
 **************************************************************************************
 
 <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.hp.fb.integration</groupId>
	<artifactId>project name</artifactId>
	<packaging>war</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>facebook-integration Maven Webapp</name>
	<url>http://maven.apache.org</url>

	<distributionManagement>
	<snapshotRepository>
	<id>nexus-snapshots</id>
	<url>http://xx.xx.xx.xxx:8081/nexus/content/repositories/snapshots/</url>
	</snapshotRepository>
	</distributionManagement>
	
	
<build>
		<plugins>

				<plugin>
						<artifactId>maven-deploy-plugin</artifactId>
						<version>2.7</version>
						<executions>
						<execution>
						<id>default-deploy</id>
						<phase>deploy</phase>
						<goals>
						<goal>deploy</goal>
						</goals>
						</execution>
						</executions>
				</plugin>
				 <plugin>
						<groupId>org.apache.maven.plugins</groupId>
						<artifactId>maven-deploy-plugin</artifactId>
						<version>2.4</version>
						<configuration>
						<skip>true</skip>
						</configuration>

				</plugin>
				

				<plugin>
					<groupId>org.codehaus.mojo</groupId>
					<artifactId>sonar-maven-plugin</artifactId>
					<version>2.3</version>
				</plugin>
				 <plugin>
						<groupId>org.sonatype.plugins</groupId>
						<artifactId>nexus-staging-maven-plugin</artifactId>
						<version>1.3</version>
						<executions>
						<execution>
						<id>default-deploy</id>
						<phase>deploy</phase>
						<goals>
						<goal>deploy</goal>
						</goals>
						</execution>
						</executions>
						<configuration>
						<serverId>nexus</serverId>
						<nexusUrl>>http://16.181.233.130:8081/nexus</nexusUrl>
						<skipStaging>true</skipStaging>
						</configuration>
				</plugin>

		</plugins>
		<finalName>project name</finalName>
</build>
 
	</project>
  ********************************************************************************
  
  
  
  
------>  global-settings.xml files as below.
  
  *************************************************************************************
 <?xml version="1.0" encoding="UTF-8"?>

<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

<!--localRepository>/var/jenkins_home/jobs</localRepository-->
<localRepository>/var/jenkins_home/.m2/repository</localRepository>
<interactiveMode>true</interactiveMode>
            
<proxies>
<!--set proxy accordingly-->
    <proxy>
      <id>optional1</id>  
      <active>true</active>
      <protocol>http</protocol>
      <host>host</host>
      <port>8080</port>
      <nonProxyHosts>*</nonProxyHosts>
    </proxy>
    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>https</protocol>
      <host>host</host>
      <port>8080</port>
      <nonProxyHosts>*.--</nonProxyHosts>
    </proxy>
  </proxies>

<servers>
 <server>
      <id>nexus.m2</id>
      <username>username</username>
      <password>password</password>
    </server>
</servers>

 
   <mirrors>    
    <mirror> 
    	<id>public</id> 
    	<mirrorOf>*</mirrorOf>     	
    	<url>http://xx.xxx.xxx.xx:8082/nexus/content/groups/public/</url>
    </mirror>		
  </mirrors>

  
<profiles><profile>
	<repositories>
		<repository>              
         	<id>central</id>
         	<name>Central</name>
         	<url>http://xx.xx.xx.xx:8082/nexus/content/repositories/aimia.releases/</url>
		</repository>
	</repositories>   
</profile></profiles>
       
</settings>

*****************************************************************************************************************
------>and in jenkins configure the job and configure nexus jenkins plugin in tht job by providing commands.
------->or we can configure maven in job and give the command as mvn clean deploy -Dmaven.test.skip=true

------->the output log contains the retrived URl of nexus repository.


 
 
 
