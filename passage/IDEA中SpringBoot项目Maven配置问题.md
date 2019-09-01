### 什么是Maven？
 Apache Maven，是一个软件（特别是Java软件）项目管理及自动构建工具，由Apache软件基金会所提供。基于项目对象模型（Project Object Model，POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤。 
 maven的项目会很小，但是也能进行和其它大项目一样的编译。这就依赖于maven项目在编译过程中所需要的jar包不在maven项目中，而是在maven的本地仓库中，maven项目中只指出所需要的jar包的坐标，等到需要用时会自动去本地仓库查找。如果存在的话就可以直接调用，如果不存在，则只需到settings.xml或项目pom.xml配置的中央仓库中下载到本地仓库即可。所以不必每次编译项目时都要准备好所需的jar包，因为本地仓库中可能早就存在了。 
  这样第一可以减少maven具体项目的大小，只需在具体项目中pom.xml文件中配置好所有的依赖关系与依赖包的坐标即可。第二编译时下载到本地仓库的jar包可以下次其它项目编译时需要相同依赖包时所用，节省了每次都要重新手动下载jar的麻烦。 

### 出现的问题：
IDEA创建了一个SpringBoot项目，里面有pom.xml文件，发现第一行报错：
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
```

### 解决方案：
查了很多资料发现我的setting.xml有问题，没法下载对应的依赖。

这里先在某个文件夹下创建一个setting.xml文件或者修改已有的setting.xml文件内容为：
 ```
<?xml version="1.0" encoding="UTF-8"?>

<!--
 Licensed to the Apache Software Foundation (ASF) under one
 or more contributor license agreements.  See the NOTICE file
 distributed with this work for additional information
 regarding copyright ownership.  The ASF licenses this file
 to you under the Apache License, Version 2.0 (the
 "License"); you may not use this file except in compliance
 with the License.  You may obtain a copy of the License at
 
 http://www.apache.org/licenses/LICENSE-2.0
 
 Unless required by applicable law or agreed to in writing,
 software distributed under the License is distributed on an
 "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
 KIND, either express or implied.  See the License for the
 specific language governing permissions and limitations
 under the License.
 -->

<!--
 | This is the configuration file for Maven. It can be specified at two levels:
 |
 |  1. User Level. This settings.xml file provides configuration for a single user,
 |                 and is normally provided in ${user.home}/.m2/settings.xml.
 |
 |                 NOTE: This location can be overridden with the CLI option:
 |
 |                 -s /path/to/user/settings.xml
 |
 |  2. Global Level. This settings.xml file provides configuration for all Maven
 |                 users on a machine (assuming they're all using the same Maven
 |                 installation). It's normally provided in
 |                 ${maven.home}/conf/settings.xml.
 |
 |                 NOTE: This location can be overridden with the CLI option:
 |
 |                 -gs /path/to/global/settings.xml
 |
 | The sections in this sample file are intended to give you a running start at
 | getting the most out of your Maven installation. Where appropriate, the default
 | values (values used when the setting is not specified) are provided.
 |
 |-->
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <!-- localRepository
     | The path to the local repository maven will use to store artifacts.
     |
     | Default: ${user.home}/.m2/repository
     <localRepository>/path/to/local/repo</localRepository>
     -->
    
    
    <!-- interactiveMode
     | This will determine whether maven prompts you when it needs input. If set to false,
     | maven will use a sensible default value, perhaps based on some other setting, for
     | the parameter in question.
     |
     | Default: true
     <interactiveMode>true</interactiveMode>
     -->
    
    <!-- offline
     | Determines whether maven should attempt to connect to the network when executing a build.
     | This will have an effect on artifact downloads, artifact deployment, and others.
     |
     | Default: false
     <offline>false</offline>
     -->
    
    <!-- pluginGroups
     | This is a list of additional group identifiers that will be searched when resolving plugins by their prefix, i.e.
     | when invoking a command line like "mvn prefix:goal". Maven will automatically add the group identifiers
     | "org.apache.maven.plugins" and "org.codehaus.mojo" if these are not already contained in the list.
     |-->
    <pluginGroups>
        <!-- pluginGroup
         | Specifies a further group identifier to use for plugin lookup.
         <pluginGroup>com.your.plugins</pluginGroup>
         -->
        <pluginGroup>org.mortbay.jetty</pluginGroup>
    </pluginGroups>
    
    <!-- proxies
     | This is a list of proxies which can be used on this machine to connect to the network.
     | Unless otherwise specified (by system property or command-line switch), the first proxy
     | specification in this list marked as active will be used.
     |-->
    <proxies>
        <!-- proxy
         | Specification for one proxy, to be used in connecting to the network.
         |
         <proxy>
         <id>optional</id>
         <active>true</active>
         <protocol>http</protocol>
         <username>proxyuser</username>
         <password>proxypass</password>
         <host>proxy.host.net</host>
         <port>80</port>
         <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
         </proxy>
         -->
    </proxies>
    
    <!-- servers
     | This is a list of authentication profiles, keyed by the server-id used within the system.
     | Authentication profiles can be used whenever maven must make a connection to a remote server.
     |-->
    <servers>
        <!-- server
         | Specifies the authentication information to use when connecting to a particular server, identified by
         | a unique name within the system (referred to by the 'id' attribute below).
         |
         | NOTE: You should either specify username/password OR privateKey/passphrase, since these pairings are
         |       used together.
         |
         <server>
         <id>deploymentRepo</id>
         <username>repouser</username>
         <password>repopwd</password>
         </server>
         -->
        
        <!-- Another sample, using keys to authenticate.
         <server>
         <id>siteServer</id>
         <privateKey>/path/to/private/key</privateKey>
         <passphrase>optional; leave empty if not used.</passphrase>
         </server>
         -->
        <server>
            <id>releases</id>
            <username>ali</username>
            <password>ali</password>
        </server>
        <server>
            <id>Snapshots</id>
            <username>ali</username>
            <password>ali</password>
        </server>
    </servers>
    
    <!-- mirrors
     | This is a list of mirrors to be used in downloading artifacts from remote repositories.
     |
     | It works like this: a POM may declare a repository to use in resolving certain artifacts.
     | However, this repository may have problems with heavy traffic at times, so people have mirrored
     | it to several places.
     |
     | That repository definition will have a unique id, so we can create a mirror reference for that
     | repository, to be used as an alternate download site. The mirror site will be the preferred
     | server for that repository.
     |-->
    <mirrors>
        <!-- mirror
         | Specifies a repository mirror site to use instead of a given repository. The repository that
         | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
         | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
         |
         <mirror>
         <id>mirrorId</id>
         <mirrorOf>repositoryId</mirrorOf>
         <name>Human Readable Name for this Mirror.</name>
         <url>http://my.repository.com/repo/path</url>
         </mirror>
         -->
        <mirror>
            <!--This sends everything else to /public -->
            <id>nexus</id>
            <mirrorOf>*</mirrorOf>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        </mirror>
        <mirror>
            <!--This is used to direct the public snapshots repo in the
             profile below over to a different nexus group -->
            <id>nexus-public-snapshots</id>
            <mirrorOf>public-snapshots</mirrorOf>
            <url>http://maven.aliyun.com/nexus/content/repositories/snapshots/</url>
        </mirror>
    </mirrors>
    
    <!-- profiles
     | This is a list of profiles which can be activated in a variety of ways, and which can modify
     | the build process. Profiles provided in the settings.xml are intended to provide local machine-
     | specific paths and repository locations which allow the build to work in the local environment.
     |
     | For example, if you have an integration testing plugin - like cactus - that needs to know where
     | your Tomcat instance is installed, you can provide a variable here such that the variable is
     | dereferenced during the build process to configure the cactus plugin.
     |
     | As noted above, profiles can be activated in a variety of ways. One way - the activeProfiles
     | section of this document (settings.xml) - will be discussed later. Another way essentially
     | relies on the detection of a system property, either matching a particular value for the property,
     | or merely testing its existence. Profiles can also be activated by JDK version prefix, where a
     | value of '1.4' might activate a profile when the build is executed on a JDK version of '1.4.2_07'.
     | Finally, the list of active profiles can be specified directly from the command line.
     |
     | NOTE: For profiles defined in the settings.xml, you are restricted to specifying only artifact
     |       repositories, plugin repositories, and free-form properties to be used as configuration
     |       variables for plugins in the POM.
     |
     |-->
    <profiles>
        <profile>
            <id>development</id>
            <repositories>
                <repository>
                    <id>central</id>
                    <url>http://central</url>
                    <releases><enabled>true</enabled><updatePolicy>always</updatePolicy></releases>
                    <snapshots><enabled>true</enabled><updatePolicy>always</updatePolicy></snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>central</id>
                    <url>http://central</url>
                    <releases><enabled>true</enabled><updatePolicy>always</updatePolicy></releases>
                    <snapshots><enabled>true</enabled><updatePolicy>always</updatePolicy></snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
        <profile>
            <!--this profile will allow snapshots to be searched when activated-->
            <id>public-snapshots</id>
            <repositories>
                <repository>
                    <id>public-snapshots</id>
                    <url>http://public-snapshots</url>
                    <releases><enabled>false</enabled></releases>
                    <snapshots><enabled>true</enabled><updatePolicy>always</updatePolicy></snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>public-snapshots</id>
                    <url>http://public-snapshots</url>
                    <releases><enabled>false</enabled></releases>
                    <snapshots><enabled>true</enabled><updatePolicy>always</updatePolicy></snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>
    
    <activeProfiles>
        <activeProfile>development</activeProfile>
        <activeProfile>public-snapshots</activeProfile>
    </activeProfiles>
</settings>

```
然后在IDEA里面进行如下配置：
![image.png](https://github.com/guangxush/iTechHeart/blob/master/image/Passage/mavenSetting.png)

然后就可以正常使用了，如果遇到其他问题或者有其他解决方案的欢迎留言！！！
### 参考链接:
[Maven介绍](https://blog.csdn.net/weixin_37987487/article/details/81665354 )
