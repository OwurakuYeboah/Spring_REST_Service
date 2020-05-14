# Spring REST Service
This is a REST service built with the Spring framework for HTTP requests.

 ## Setting up the Database

These instructions will get you a copy of the database up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the database on a live system.

### Prerequisites

You will need:

SQL Server 2017 Express Edition(can upgrade to 2019). Make sure you download the Management Studio as well. Here's a link to install:

https://www.microsoft.com/en-us/sql-server/sql-server-downloads


### Installing

Download the BACPAC file from this repo.

Open your SQL Server Management Studio.

After signing into your desktop, right-click on 'Databases' in the Object Explorer.

Click the 'Import Data-tier Application...' option

Follow the instructions to import the BACPAC file into a new database.

Your database is now set up! You can change your SQL authentication to match up with the service layer.

## Running the tests

You can run queries on the database by right-clicking the new database and clicking 'New Query'


## Deployment

These are the steps to take to deploy the database onto Azure:

First, you must create an Azure SQL database to deploy the database onto. Refer to the Azure documentation for the instructions.

After your Azure SQL Database is created, open your SQL Server Management Studio.

Right-click on the database created, and go to Tasks.

Select the 'Deploy Database to Azure SQL Database' option.

The wizard will walk you through deploying your database onto Azure.

Your database is now deployed on Azure! You can also run queries from your SQL Server Management Studio. (option provided in wizard)


## Setting up the REST service

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

You will need:
An existing instance of a SQL Server database. Refer to the "Setting up the database" section of the README.

Eclipse (or any Java IDE you prefer). Here's a link to install:

https://www.eclipse.org/downloads/



### Installing

Download the zip file and export.

After Eclipse is done installing, import the exported folder as a project.

Locate for the applications.properties file in src/main/resources. The file should look like this:


```
spring.datasource.url=jdbc:sqlserver://localhost;databaseName=*YOUR DATABASE NAME GOES HERE*
spring.datasource.username=*YOUR USERNAME GOES HERE*
spring.datasource.password=*YOUR PASSWORD GOES HERE*
spring.datasource.driverClassName=com.microsoft.sqlserver.jdbc.SQLServerDriver
```

Here, you can change the database name, username and password to fit your SQL Server database.

You are now ready to run!



## Running Locally

Run the project on your IDE. In your console log, you should see a Started a Spring service in seconds message.

Your service is now running! Open your internet browser, and type:

```
localhost:8080/patientobjects
```

You should see a JSON object with the patient's data. This means your REST API is up and running!

This is the navigation through the API:

```
*url*/patientobjects - Display all patient objects in a JSON
*url*/patientobjects/patientid/*id* - Display a single patient based on ID
```


## Deployment

These are the steps you take to deploy to Azure, using GitHub:

First, you must create an API app service on your Azure portal. Refer to Azure documentation.

In the application.properties file, you must change it to this:
```
spring.datasource.url=jdbc:sqlserver://*SERVER NAME GOES HERE*.database.windows.net:1433;database=*DATABASE NAME GOES HERE;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
spring.datasource.username=*YOUR USERNAME GOES HERE*
spring.datasource.password=*YOUR PASSWORD GOES HERE*
spring.datasource.driverClassName=com.microsoft.sqlserver.jdbc.SQLServerDriver
```
This information can be retrieved from 'Connection Strings' in the 'Overview' page of your Azure SQL Database.

In your pom.xml file, under the parent tag on line 12, add the following line:
```
<packaging>jar</packaging>
```

Then under the plugins tag, add the following plugin:
```
<plugin> 
        <groupId>com.microsoft.azure</groupId>  
        <artifactId>azure-webapp-maven-plugin</artifactId>  
        <version>1.9.0</version>  
        <configuration>
          <resourceGroup>*YOUR RESOURCE GROUP NAME GOES HERE*</resourceGroup>
          <appName>*YOUR APP NAME GOES HERE*</appName>
          <region>*YOUR REGION GOES HERE*</region>
          <runtime>
            <os>linux</os>
            <javaVersion>jre8</javaVersion>
            <webContainer>jre8</webContainer>
          </runtime>
          <appSettings>
          <property>
                <name>JAVA_OPTS</name>
                <value>-Dserver.port=80</value>
          </property>
       </appSettings>
          <deployment>
            <resources>
              <resource>
                <directory>${project.basedir}/target</directory>
                <includes>
                  <include>*.jar</include>
                </includes>
              </resource>
            </resources>
          </deployment>
        </configuration>
      </plugin>
```

On your Package explorer, right-click your project.

Go to Run As, and click Maven install.

This will create a JAR file. The location of the file will be in the console log.

Now, push this JAR file into a repository on GitHub. This repository will be used as a deployment center.

Go to your created Azure App Service, and set up your deployment center to use GitHub. A detailed tutorial is found here:

https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/app-service/deploy-continuous-deployment.md

After deployment is complete, your service layer is currently running on Azure! Your URL should be on your 'Overview' tab.



## Built With

* [SQL Server 2017](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
* [Spring Boot](https://spring.io/projects/spring-boot) - Used to build REST API
* [Maven](https://maven.apache.org/) - Dependency Management
* [Java v1.8](https://www.oracle.com/java/technologies/javase-jdk8-downloads.html)

