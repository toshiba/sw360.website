---
linkTitle: "Native-Install 14 and Upgrade 15"
title: "Native-Install 14 and Upgrade 15"
weight: 100
description: 
  Native-Install-Version-14.0.0-and-Upgrade-Version-15.0.0
---

# SW360 Version up test

## 1. Overview
### 1.1 SW360 Portal

A software component catalogue application - designed to work with FOSSology.

SW360 is a server with a REST interface and a Liferay CE portal application to maintain your projects / products and the software components within.
It can manage SPDX files for maintaining the license conditions and maintain license information.

This material helps user to install SW360 14.0 and how to upgrade SW360 from 14.0 to 15.0.

### 1.2 Environment

| Package Name  | Version  | 
|:--------------|:--------:|
|   Ubuntu      |  18.04   |
|   Apt         |  1.6.14  |
|   Wget        |  1.19.4  |
|   Curl        |  7.58.0  |
|   Git         |  2.17.1  |
|   Maven       |  3.6.0   |
|   OpenJDK     |   11     |

## 2.Install & Config proxy for Environment
```
2.1 Apt
2.2 Wget
2.3 Curl
2.4 Git
2.5 Maven
2.6 OpenJDK 
```
### 2.1 Apt
#### Create file with name proxy.conf in folder `/etc/apt/apt.conf.d`

   - `$  sudo gedit /etc/apt/apt.conf.d/proxy.conf`

#### Add the following line few files `proxy.conf` 
```
    Acquire {
        HTTP::proxy "http://username:password@server:port";
        HTTPS::proxy "http://username:password@server:port";
    }
```
### 2.2 Wget
#### Create file `~/.wgetrc`

   - `$  sudo gedit ~/.wgetrc`

####  Add the following line few files `~/.wgetrc` 
```
	use_proxy=yes
	  http_proxy=http://username:password@server:port
	  https_proxy=http://username:password@server:port
```
### 2.3 Curl
#### 2.3.1 Install Curl
   - `$ sudo apt update`
   - `$ sudo apt install curl`

#### 2.3.2 Config proxy
* Create file `~/.curlrc`

   - `$  sudo gedit ~/.curlrc`

*  Add the following line few files `~/.curlrc` 
```
    proxy=http://username:password@server:port/
```

### 2.4 Git

#### 2.4.1 Install Git
-   `$ sudo apt update`
-   `$ sudo apt install git`
#### 2.4.2 Config proxy
* Create file `~/.gitconfig`

   - `$  sudo gedit ~/.gitconfig`

* Add the following line few files `~/.gitconfig`
   ```
    [http]
        proxy = http://username:password@server:port
        sslverify = false
    [https]
        proxy = http://username:password@server:port

   ```     
### 2.5 Maven
#### 2.5.1 Install Maven
*Go to back Home in Terminal

-   `$ sudo apt update`
-   `$ sudo apt install maven`

#### 2.5.2 Config proxy for Maven

* Create Folder with  path `/home/user/.m2`
-   `$ mkdir /home/user/.m2`

* Create File in Folder `.m2` 
-   `$ touch /home/user/.m2/settings.xml`

* Copy the following lines into tag <proxies></proxies>
        
            <settings>
                <proxies>
                    <proxy>
                    
                        <id>optional1</id>

                        <active>true</active>       

                        <protocol>http</protocol>
                        
                        <username>username</username>
                        
                        <password>password</password>
                        
                        <host>server</host>

                        <port>port</port>
                        
                        <nonProxyHosts>local.net</nonProxyHosts>
                    
                    </proxy>

                    <proxy>
                    
                       <id>optional1</id>

                        <active>true</active>       

                        <protocol>http</protocol>
                        
                        <username>username</username>
                        
                        <password>password</password>
                        
                        <host>server</host>
                        
                        <port>port</port>
                        
                        <nonProxyHosts>local.net</nonProxyHosts>
                    
                    </proxy>
                </proxies>
            </settings>
### 2.6 OpenJDK 11 

* And install OpenJDK 11
    - `$ sudo apt install openjdk-11-jdk`
* Check version: 
    - `$ java --version`
    - Output: 
    ```
          openjdk version "11.0.15" 2022-04-19
          OpenJDK Runtime Environment (build 11.0.15+10-Ubuntu-0ubuntu0.18.04.1)
          OpenJDK 64-Bit Server VM (build 11.0.15+10-Ubuntu-0ubuntu0.18.04.1, mixed mode, sharing)
    ```
    - Install JDK successfully                
## 3. Native install 14.0 (without docker-compose)

### The installation consists of some tasks::
```
3.1 Install A Liferay Community Edition bundled with Tomcat and download dependencies as OSGi modules
3.2 Install Couchdb version 3.2.2 
3.3 Install Couchdb Lucene
3.4 Clone Project sw360 with version 14
3.5 Install Thrift version 14.0 
3.6 Compiling and deploying
3.7 Version Management Table
```
### 3.1 Install A Liferay Community Edition bundled with Tomcat

* Make folder `work` in path of work: `/home/user`

    - `$ mkdir work` 
    
* Download Liferay Portal CE 7.3.4 GA5
    - `$ cd work`
    - `$ wget  --no-check-certificate https://sourceforge.net/projects/lportal/files/Liferay%20Portal/7.3.4%20GA5/liferay-ce-portal-tomcat-7.3.4-ga5-20200811154319029.tar.gz/download -O liferay-ce-portal-tomcat-7.3.4-ga5.tar.gz`

* Extract downloaded file
    - `$ tar -xzf liferay-ce-portal-tomcat-7.3.4-ga5.tar.gz`

* Set Environment for `${LIFERAY_INSTALL}`
    - `$ export LIFERAY_INSTALL=/opt/liferay-ce-portal-7.3.4-ga5`

*  Move to `${LIFERAY_INSTALL}/deploy` and Run command: 

    * `$ cd ${LIFERAY_INSTALL}/deploy`

    * ` wget https://search.maven.org/remotecontent?filepath=commons-codec/commons-codec/1.12/commons-codec-1.12.jar -O commons-codec-1.12.jar `
    * ` wget https://search.maven.org/remotecontent?filepath=org/apache/commons/commons-collections4/4.4/commons-collections4-4.4.jar -O commons-collections4-4.4.jar `
    * ` wget https://search.maven.org/remotecontent?filepath=org/apache/commons/commons-csv/1.4/commons-csv-1.4.jar -O commons-csv-1.4.jar `
    * ` wget https://search.maven.org/remotecontent?filepath=commons-io/commons-io/2.6/commons-io-2.6.jar -O commons-io-2.6.jar`
    * ` wget https://search.maven.org/remotecontent?filepath=commons-lang/commons-lang/2.4/commons-lang-2.4.jar -O commons-lang-2.4.jar`
    * ` wget https://search.maven.org/remotecontent?filepath=commons-logging/commons-logging/1.2/commons-logging-1.2.jar -O commons-logging-1.2.jar`
    * ` wget https://search.maven.org/remotecontent?filepath=com/google/code/gson/gson/2.8.5/gson-2.8.5.jar -O gson-2.8.5.jar`
    * ` wget https://search.maven.org/remotecontent?filepath=com/google/guava/guava/21.0/guava-21.0.jar -O guava-21.0.jar`
    * ` wget https://search.maven.org/remotecontent?filepath=com/fasterxml/jackson/core/jackson-annotations/2.11.3/jackson-annotations-2.11.3.jar -O jackson-annotations-2.11.3.jar`
    * ` wget https://search.maven.org/remotecontent?filepath=com/fasterxml/jackson/core/jackson-core/2.11.3/jackson-core-2.11.3.jar -O jackson-core-2.11.3.jar`
    * ` wget https://search.maven.org/remotecontent?filepath=com/fasterxml/jackson/core/jackson-databind/2.11.3/jackson-databind-2.11.3.jar -O jackson-databind-2.11.3.jar`
    * ` wget https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.20/commons-compress-1.20.jar -O commons-compress-1.20.jar`
    * ` wget https://repo1.maven.org/maven2/org/apache/thrift/libthrift/0.13.0/libthrift-0.13.0.jar -O libthrift-0.13.0.jar`  


* Create `portal-ext.properties` file in `liferay-ce-portal-7.3.4-ga5` folder

* Copy content from  https://github.com/eclipse/sw360/blob/sw360-14.0.0-M1/frontend/configuration/portal-ext.properties to portal-ext.properties

- Edit `portal-ext.properties`: uncomment below lines  

        # default.admin.password=sw360fossy

        # default.admin.screen.name=setup

        # default.admin.email.address.prefix=setup
  
        # default.admin.first.name=Setup
    
        # default.admin.last.name=Administrator

* Remove files in folder `hypersonic` with path: `/home/user/work/liferay-ce-portal-7.3.4-ga5/data/hypersonic`
    - `$ rm -rf /home/user/work/liferay-ce-portal-7.3.4-ga5/data/hypersonic/*`

* Create folder `tosspedia` in path `/opt/tosspedia`

    - `$ sudo mkdir tosspedia`

* Move folder `liferay-ce-portal-7.3.4-ga5` to `/opt/tosspedia`

    - `$ sudo mv liferay-ce-portal-7.3.4-ga5 /opt/tosspedia`

### 3.2 Install Couchdb version 3.2.2 

* Run the following commands:
    - `$ sudo apt update && sudo apt install -y curl apt-transport-https gnupg`
    - `$ curl https://couchdb.apache.org/repo/keys.asc | gpg --dearmor | sudo tee /usr/share/keyrings/couchdb-archive-keyring.gpg >/dev/null 2>&1 `
    - `$ source /etc/os-release `
    - `$ echo "deb [signed-by=/usr/share/keyrings/couchdb-archive-keyring.gpg] https://apache.jfrog.io/artifactory/couchdb-deb/ ${VERSION_CODENAME} main" \ `
    ` | sudo tee /etc/apt/sources.list.d/couchdb.list >/dev/null `
    - `$ sudo apt update `
    - `$ sudo apt install -y couchdb `


* Config and Setup Couchdb follow images:
    * Config Couchdb Type  
    &nbsp;
    {{< gallery/gallery >}} 

        {{< gallery/card src="/sw360/img/couchdb/ConfigTypeCouchdb.jpg" title="Type Couchdb" >}}
        
        {{< /gallery/card >}}

    {{< /gallery/gallery >}}

    <!-- ![](./assets/img/ConfigTypeCouchdb.jpg ) -->
    &nbsp;    
    * Config node name  
    &nbsp;
    {{< gallery/gallery >}} 

        {{< gallery/card src="/sw360/img/couchdb/ConfigDomainCouchdb.jpg" title="Domain Couchdb" >}}
        
        {{< /gallery/card >}}

    {{< /gallery/gallery >}}
    <!-- ![](./assets/img/ConfigDomainCouchdb.jpg) -->
    * Set up magic cookie  
    &nbsp;
    {{< gallery/gallery >}} 

        {{< gallery/card src="/sw360/img/couchdb/SetupMagicCookie.jpg" title="Setup Magic Cookie" >}}
        
        {{< /gallery/card >}}

    {{< /gallery/gallery >}}
    <!-- ![](./assets/img/SetupMagicCookie.jpg) -->
    * Config bind address  
    &nbsp;
    {{< gallery/gallery >}} 

        {{< gallery/card src="/sw360/img/couchdb/ConfigIPCouchdb.jpg" title="IP Couchdb" >}}
        
        {{< /gallery/card >}}

    {{< /gallery/gallery >}}
    <!-- ![](./assets/img/ConfigIPCouchdb.jpg) -->
    * Set up Couchdb Password  
    &nbsp;
    {{< gallery/gallery >}} 

        {{< gallery/card src="/sw360/img/couchdb/SetupPassword.jpg" title="Setup Password" >}}
        
        {{< /gallery/card >}}

    {{< /gallery/gallery >}}
    <!-- ![](./assets/img/SetupPassword.jpg) -->
    * Repeat Password  
    &nbsp;
    {{< gallery/gallery >}} 

        {{< gallery/card src="/sw360/img/couchdb/RepeatPassword.jpg" title="Repeat Password" >}}
        
        {{< /gallery/card >}}

    {{< /gallery/gallery >}}
    <!-- ![](./assets/img/RepeatPassword.jpg) -->
    * Start and status of Couchdb
    &nbsp;
    {{< gallery/gallery >}} 

        {{< gallery/card src="/sw360/img/couchdb/StatusStartCouchdb.jpg" title="Status Start Couchdb" >}}
        
        {{< /gallery/card >}}

    {{< /gallery/gallery >}}
    <!-- ![](./assets/img/StatusStartCouchdb.jpg) -->
    * Stop and status of Couchdb
    &nbsp;
    {{< gallery/gallery >}} 

        {{< gallery/card src="/sw360/img/couchdb/StatusStopCouchdb.jpg" title="Status Stop Couchdb" >}}
        
        {{< /gallery/card >}}

    {{< /gallery/gallery >}}
    <!-- ![](./assets/img/StatusStopCouchdb.jpg)  -->

* Url and account when config couchdb 
    - Username: `admin`
    - Password: `password`
    - Url:  `http://localhost:5984/_utils`

* Command Line When Start, Stop, check status Couchdb 
    - Start Couchdb: `$ sudo service couchdb start`
    - Stop Couchdb: `$ sudo service couchdb stop`
    - Check status: `$ sudo service couchdb status`
### 3.3 Install Couchdb Lucene

* SW360 uses for searching the contents of the couchdb databases a lucene-based server named couchdb-lucene

* Run command download Couchdb Lucene
    - `wget --no-check-certificate https://github.com/rnewson/couchdb-lucene/archive/v2.1.0.tar.gz -O couchdb-lucene.tar.gz`

* Note Extract liferay To folder `work` with path of work: `/home/user/work`
    - `tar -xzf couchdb-lucene.tar.gz`

* Run command: 
    - `cd couchdb-lucene-2.1.0`
    - `sed -i "s/allowLeadingWildcard=false/allowLeadingWildcard=true/" ./src/main/resources/couchdb-lucene.ini `
    - `sed -i "s/localhost:5984/admin:password@localhost:5984/" ./src/main/resources/couchdb-lucene.ini `
    - `wget https://raw.githubusercontent.com/sw360/sw360vagrant/master/shared/couchdb-lucene.patch `
    - `patch -p1 < couchdb-lucene.patch `
    - `mvn clean install war:war`
    - `cp target/couchdb-lucene-*.war /opt/liferay-ce-portal-7.3.4-ga5/tomcat-9.0.33/webapps/couchdb-lucene.war`
    

### 3.4 Clone sw360 with version 14.0.0

* Clone sw360 source code to folder `work` with path: `/home/user/work`

    - `$ git clone https://github.com/eclipse/sw360`

* Checkout to tag 14.0.0 version
    - `$ cd sw360`
    - `$ git checkout  sw360-14.0.0-M1`

### 3.5 Install Thrift version 0.14 

* For thrift, we need version 0.14. The installation script in Path: `SW360_REPOSITORY/scripts/install-thrift.sh`

* Run command:
    - `$ chmod +x install-thrift.sh`
    - `$ sudo ./install-thrift.sh`

In case there is thrift in the package management of the OS you re running on, just make sure, you have version 0.14
* Check version thrift

    - `$ thrift --version`
    
    - Output: 
    ```
        Thrift version 0.14.0

    ```
    - Install Thrift successfully     
### 3.6 Compile and deploy

* Start couchdb
    - `$ sudo service couchdb start`

* Set Environment for `${LIFERAY_INSTALL}`
    - `$ cd /home/user/work/sw360`
    - `$ export LIFERAY_INSTALL=/opt/liferay-ce-portal-7.3.4-ga5`

1. To clean everything and  install without running the tests
    - `$ mvn clean install -DskipTests `

2. For deployment run the command 
    - `mvn package -P deploy -Dbase.deploy.dir=. -Dliferay.deploy.dir=${LIFERAY_INSTALL}/deploy -Dbackend.deploy.dir=${LIFERAY_INSTALL}/tomcat-9.0.33/webapps -Drest.deploy.dir=${LIFERAY_INSTALL}/tomcat-9.0.33/webapps -DskipTests`
    
#### 3.6.1 Start and Configure Liferay 
* Set Environment for `${LIFERAY_INSTALL}`
    - `$ export LIFERAY_INSTALL=/opt/liferay-ce-portal-7.3.4-ga5`

* Start liferay    
    - `$ ${LIFERAY_INSTALL}/tomcat-9.0.33/bin/startup.sh`
* Log    
    - `$ tail -f ${LIFERAY_INSTALL}/tomcat-9.0.33/logs/*`

* Url SW360 : `https://localhost:8080`
#### 3.6.2 Configure Liferay Portal

* Can follow the steps in the following link https://www.eclipse.org/sw360/docs/deployment/legacy/deploy-liferay7.3 or follow these steps:

- Import users
    1. 	Open the panel on the left side by clicking the button on the top left.
    2. 	Click on `SW360` on the top right to go to the homepage.
    3.	Click on `Start` inside the "Welcome" section.
    4.	Go to `Admin` -> `User` (URL: `/group/guest/users`).
    5.	Scroll down to section `UPLOAD USERS`, select a user file from the very
        beginning and click `Upload Users` on the right side. [A user file can be found here in the sw360vagrant project](https://github.com/sw360/sw360vagrant/blob/master/shared/test_users_with_passwords_12345.csv)
        * Download: `$ wget https://github.com/sw360/sw360vagrant/blob/master/shared/test_users_with_passwords_12345.csv`


### 3.7 Version Management Table (sw360 14.0)

| Package Name  | Version  | 
|:--------------|:--------:|
|   Liferay     |  7.3.4   |
|   Tomcat      |  9.0.33  |
|   Couchdb     |  3.2.2   |
|   Open JDK    |  11.0.15 |
|   Thrift      |  0.14.0  |
|   SW360       |  14.0.0  |



### 3.8 Config couchdb with Sw360 (sw360 14.0)

* Create folder `sw360` in path `/etc/`

    - `sudo mkdir sw360`

* Create 2 folder `authorization` and `rest` in path `/etc/sw360`

    - `sudo mkdir authorization`
    - `sudo mkdir rest`

*  Create file `application.yml` in path `/etc/sw360/authorizaton` with content: 
```
        #
        # Copyright Siemens AG, 2017, 2019. Part of the SW360 Portal Project.
        #
        # This program and the accompanying materials are made
        # available under the terms of the Eclipse Public License 2.0
        # which is available at https://www.eclipse.org/legal/epl-2.0/
        #
        # SPDX-License-Identifier: EPL-2.0
        #

        # Port to open in standalone mode
        server:
        port: 8090

        # Connection to the couch databases. Will be used to store client credentials
        couchdb:
        url: http://localhost:5984
        database: sw360oauthclients
        # if your couchdb does not use authentication, pls just don't use the settings for username and password
        username: admin
        password: password

        jwt:
        secretkey: sw360SecretKey

        spring:
        jackson:
            serialization:
            indent_output: true

        # Common SW360 properties
        sw360:
        # The url of the Liferay instance
        sw360-portal-server-url: ${SW360_PORTAL_SERVER_URL:http://127.0.0.1:8080}
        # The id of the company in Liferay that sw360 is run for
        sw360-liferay-company-id: ${SW360_LIFERAY_COMPANY_ID:20101}
        # Allowed origins that should be set in the header
        cors:
            allowed-origin: ${SW360_CORS_ALLOWED_ORIGIN:#{null}}

        security:
        # Configuration for enabling authorization via headers, e.g. when using SSO
        # in combination with a reverse proxy server
        customheader:
            headername:
            # You have to enable authorization by headers explicitly here
            enabled: false
            # Attention: please make sure that the proxy is removing there headers
            # if they are coming from anywhere else then the authentication server
            intermediateauthstore: custom-header-auth-marker
            email: authenticated-email
            extid: authenticated-extid
            # also available - at least in saml pre auth - are "givenname", "surname" and "department"

        oauth2:
            resource:
            id: sw360-REST-API

```
*  Create file `application.yml` in path `/etc/sw360/rest` with content:
```
        #
        # Copyright Siemens AG, 2017. Part of the SW360 Portal Project.
        # Copyright Bosch.IO GmbH 2020
        #
        # This program and the accompanying materials are made
        # available under the terms of the Eclipse Public License 2.0
        # which is available at https://www.eclipse.org/legal/epl-2.0/
        #
        # SPDX-License-Identifier: EPL-2.0
        #

        server:
        port: 8091

        management:
        endpoints:
            enabled-by-default: false
            web:
            base-path:
        endpoint:
            health:
            enabled: true
            show-details: always
            info:
            enabled: true
            web:
            base-path: /

        spring:
        servlet:
            multipart:
            max-file-size: 500MB
            max-request-size: 600MB

        # logging:
        #   level:
        #     org.springframework.web: DEBUG

        security:
        oauth2:
            resource:
            id: sw360-REST-API
            jwt:
                keyValue: |
                -----BEGIN PUBLIC KEY-----
                MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEApz8Cr1o5yHMv/FUdF5uy
                VptilqdWtNvw5S6Tr4IaQ4XR9QPt8nlRsjOngfG4QCcKMBWJISldFg8PlJWUBeV+
                6TwQUidxokl2GbO6/+QA+lz1a5Ei1Y1pcnvFeRb2pdYlH3Yg6fXMxS6QwDLk27pZ
                5xbpSDIGISDesyaIMvwaKdhAbFW/tTb/oJY7rCPvmYLT80kJzilijJ/W01jMMSHg
                9Yi5cCt1eU/s78co+pxHzwNXO0Ul4iRpo/CXprQCsSIsdWkJTo6btal1xzd292Da
                d+9xq499JEsNbcqLfCq8DBQ7CEz6aJjMvPkvZiCrFIGxC/Gqmw35DQ4688rbkKSJ
                PQIDAQAB
                -----END PUBLIC KEY-----

        sw360:
        thrift-server-url: ${SW360_THRIFT_SERVER_URL:http://localhost:8080}
        test-user-id: admin@sw360.org
        test-user-password: sw360-password
        couchdb-url: ${SW360_COUCHDB_URL:http://localhost:5984}
        cors:
            allowed-origin: ${SW360_CORS_ALLOWED_ORIGIN:#{null}}
```

* Create file `couchdb.properties` in path `/etc/sw360` with content:

```
        #
        # Copyright Siemens AG, 2020. Part of the SW360 Portal Project.
        #
        # This program and the accompanying materials are made
        # available under the terms of the Eclipse Public License 2.0
        # which is available at https://www.eclipse.org/legal/epl-2.0/
        #
        # SPDX-License-Identifier: EPL-2.0
        #

        couchdb.url = http://localhost:5984
        couchdb.user = admin
        couchdb.password = password
        couchdb.database = sw360db
        couchdb.usersdb = sw360users
        couchdb.attachments = sw360attachments
        lucenesearch.limit = 10000

```
* Create file `sw360.properties` and `/etc/sw360` with content:

```
        #
        # Copyright Siemens AG, 2020. Part of the SW360 Portal Project.
        #
        # This program and the accompanying materials are made
        # available under the terms of the Eclipse Public License 2.0
        # which is available at https://www.eclipse.org/legal/epl-2.0/
        #
        # SPDX-License-Identifier: EPL-2.0
        #

        rest.apitoken.generator.enable = true
        #custom.welcome.page.guideline = true
        sw360.liferay.company.id=20098

        rest.write.access.usergroup=USER
        project.obligations.enabled=true

        ## Timeout for connecting to the backend
        backend.timeout.connection = 50000

        ## Timeout for reading response from the backend
        ## This defines the maximum wait time until the
        ## first byte of the response is available.
        backend.timeout.read = 6000000

        #Log
        enable.sw360.change.log=true
        sw360changelog.config.file.location=/opt/sw360logs

        #auto.set.ecc.status = false
        #component.visibility.restriction.enabled=true

```
## 4. Upgrade sw360 from 14.0 to 15.0
```
4.1 Checkout source Code SW360 to Tag Version 15
4.2 Version of libraries
4.3 Migrate database
4.4 Build and deploy Sw360 Version 15.0
```
### 4.1 Checkout source Code SW360 to Tag Version 15 

Link contains source: <https://github.com/eclipse/sw360.git>

*   Path `SW360_REPOSITORY` = `/home/user/work/sw360`

*   Source code sw360 is in master branch with commit version 14.0 . User into `${SW360_REPOSITORY}` use git checkout to tag version 15 on the master branch of SW360
*   Checkout to tag  Version 15.0.0
    - `$ git checkout . `
    - `$ git checkout sw360-15.0.0-M1`

### 4.2 Version of libraries

* With libraries same `3.7 section`

### 4.3 Migrate database

* Check migrate scripts from 14.0 to 15.0 by <https://github.com/eclipse/sw360/tree/master/scripts/migrations> 
    
-  There is no migrate script, skip this step. 

### 4.4 Build and deploy SW360 Version 15.0

* Set Environment for `${LIFERAY_INSTALL}`
    - `$ export LIFERAY_INSTALL=/opt/liferay-ce-portal-7.3.4-ga5`

* Stop SW360 version 14.0, ensure that couchdb is accessible (try to open `http://localhost:5984/_utils/`)
    - `$ ${LIFERAY_INSTALL}/tomcat-9.0.33/bin/shutdown.sh`

* Configure couchdb: same `3.4 section`
* Compile and deploy: same `3.6 section`