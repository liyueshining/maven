# maven pom.xml详解（转）

## 一、什么是POM
Project Object Model，项目对象模型。通过xml格式保存的pom.xml文件。作用类似ant的build.xml文件，功能更强大。该文件用于管理：源代码、配置文件、开发者的信息和角色、问题追踪系统、组织信息、项目授权、项目的url、项目的依赖关系等等。
一个完整的pom.xml文件，放置在项目的根目录下。


```html
  <project xmlns="http://maven.apache.org/POM/4.0.0"  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0  
                      http://maven.apache.org/maven-v4_0_0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <!– The Basics –>  
  <groupId>…</groupId>  
  <artifactId>…</artifactId>  
  <version>…</version>  
  <packaging>…</packaging>  
  <dependencies>…</dependencies>  
  <parent>…</parent>  
  <dependencyManagement>…</dependencyManagement>  
  <modules>…</modules>  
  <properties>…</properties>  
  <!– Build Settings –>  
  <build>…</build>  
  <reporting>…</reporting>  
  <!– More Project Information –>  
  <name>…</name>  
  <description>…</description>  
  <url>…</url>  
  <inceptionYear>…</inceptionYear>  
  <licenses>…</licenses>  
  <organization>…</organization>  
  <developers>…</developers>  
  <contributors>…</contributors>  
  <!– Environment Settings –>  
  <issueManagement>…</issueManagement>  
  <ciManagement>…</ciManagement>  
  <mailingLists>…</mailingLists>  
  <scm>…</scm>  
  <prerequisites>…</prerequisites>  
  <repositories>…</repositories>  
  <pluginRepositories>…</pluginRepositories>  
  <distributionManagement>…</distributionManagement>  
  <profiles>…</profiles>  
</project>  
```


## 二、基本设置

### 1、maven的协作相关属性
```html 
<project xmlns="http://maven.apache.org/POM/4.0.0"  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0  
                      http://maven.apache.org/maven-v4_0_0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>org.codehaus.mojo</groupId>  
  <artifactId>my-project</artifactId>  
  <version>1.0</version>  
  <packaging>war</packaging>  
</project>
```
groupId : 组织标识，例如：org.codehaus.mojo，在M2_REPO目录下，将是: org/codehaus/mojo目录。
artifactId : 项目名称，例如：my-project，在M2_REPO目录下，将是：org/codehaus/mojo/my-project目录。
version : 版本号，例如：1.0，在M2_REPO目录下，将是：org/codehaus/mojo/my-project/1.0目录。
packaging : 打包的格式，可以为：pom , jar , maven-plugin , ejb , war , ear , rar , par

### 2、POM之间的关系

主要用于POM文件的复用。
a）依赖关系：依赖关系列表（dependency list）是POM的重要部分
```html 
  <dependencies>  
   <dependency>  
     <groupId>junit</groupId>  
     <artifactId>junit</artifactId>  
     <version>4.0</version>  
     <scope>test</scope>  
   </dependency>  
   …  
 </dependencies>
 ```
groupId , artifactId , version :
scope : compile(default),provided,runtime,test,system
exclusions
b）继承关系：继承其他pom.xml配置的机制。
比如父pom.xml：

```html
<project>  
  [...]  
  <dependencies>  
    <dependency>  
      <groupId>junit</groupId>  
      <artifactId>junit</artifactId>  
      <version>4.4</version>  
      <scope>test</scope>  
    </dependency>  
  </dependencies>  
  [...]  
</project>
```
在子pom.xml文件继承它的依赖（还可以继承其他的：developers and contributors、plugin lists、reports lists、plugin executions with matching ids、plugin configuration）：
```html 
[...]  
<parent>  
<groupId>com.devzuz.mvnbook.proficio</groupId>  
  <artifactId>proficio</artifactId>  
  <version>1.0-SNAPSHOT</version>  
</parent>  
[...]
```
在这种机制下，maven还提供了一个类似java.lang.Object的顶级父pom.xml文件：
```html 
<project>  
  <modelVersion>4.0.0</modelVersion>  
  <name>Maven Default Project</name>  
  <repositories>  
    <repository>  
      <id>central</id>  
      <name>Maven Repository Switchboard</name>  
      <layout>default</layout>  
      <url>http://repo1.maven.org/maven2</url>  
      <snapshots>  
        <enabled>false</enabled>  
      </snapshots>  
    </repository>  
  </repositories>  
  <pluginRepositories>  
    <pluginRepository>  
      <id>central</id>  
      <name>Maven Plugin Repository</name>  
      <url>http://repo1.maven.org/maven2</url>  
      <layout>default</layout>  
      <snapshots>  
        <enabled>false</enabled>  
      </snapshots>  
      <releases>  
        <updatePolicy>never</updatePolicy>  
      </releases>  
    </pluginRepository>  
  </pluginRepositories>  
  <build>  
    <directory>target</directory>  
    <outputDirectory>target/classes</outputDirectory>  
    <finalName>${project.artifactId}-${project.version}</finalName>  
    <testOutputDirectory>target/test-classes</testOutputDirectory>  
    <sourceDirectory>src/main/java</sourceDirectory>  
    <scriptSourceDirectory>src/main/scripts</scriptSourceDirectory>  
    <testSourceDirectory>src/test/java</testSourceDirectory>  
    <resources>  
      <resource>  
        <directory>src/main/resources</directory>  
      </resource>  
    </resources>  
    <testResources>  
      <testResource>  
        <directory>src/test/resources</directory>  
      </testResource>  
    </testResources>  
    <pluginManagement>  
       <plugins>  
         <plugin>  
           <artifactId>maven-antrun-plugin</artifactId>  
           <version>1.1</version>  
         </plugin>        
         <plugin>  
           <artifactId>maven-assembly-plugin</artifactId>  
           <version>2.2-beta-2</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-clean-plugin</artifactId>  
           <version>2.2</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-compiler-plugin</artifactId>  
           <version>2.0.2</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-dependency-plugin</artifactId>  
           <version>2.0</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-deploy-plugin</artifactId>  
           <version>2.3</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-ear-plugin</artifactId>  
           <version>2.3.1</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-ejb-plugin</artifactId>  
           <version>2.1</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-install-plugin</artifactId>  
           <version>2.2</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-jar-plugin</artifactId>  
           <version>2.2</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-javadoc-plugin</artifactId>  
           <version>2.4</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-plugin-plugin</artifactId>  
           <version>2.4.1</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-rar-plugin</artifactId>  
           <version>2.2</version>  
         </plugin>  
         <plugin>                 
           <artifactId>maven-release-plugin</artifactId>  
           <version>2.0-beta-7</version>  
         </plugin>  
         <plugin>                 
           <artifactId>maven-resources-plugin</artifactId>  
           <version>2.2</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-site-plugin</artifactId>  
           <version>2.0-beta-6</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-source-plugin</artifactId>  
           <version>2.0.4</version>  
         </plugin>           
         <plugin>  
            <artifactId>maven-surefire-plugin</artifactId>  
            <version>2.4.2</version>  
         </plugin>  
         <plugin>  
           <artifactId>maven-war-plugin</artifactId>  
           <version>2.1-alpha-1</version>  
         </plugin>  
       </plugins>  
     </pluginManagement>  
  </build>  
  <reporting>  
    <outputDirectory>target/site</outputDirectory>  
  </reporting>  
  <profiles>  
    <profile>  
      <id>release-profile</id>  
      <activation>  
        <property>  
          <name>performRelease</name>  
          <value>true</value>  
        </property>  
      </activation>  
      <build>  
        <plugins>  
          <plugin>  
            <inherited>true</inherited>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-source-plugin</artifactId>  
            <executions>  
              <execution>  
                <id>attach-sources</id>  
                <goals>  
                  <goal>jar</goal>  
                </goals>  
              </execution>  
            </executions>  
          </plugin>  
          <plugin>  
            <inherited>true</inherited>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-javadoc-plugin</artifactId>  
            <executions>  
              <execution>  
                <id>attach-javadocs</id>  
                <goals>  
                  <goal>jar</goal>  
                </goals>  
              </execution>  
            </executions>  
          </plugin>  
          <plugin>  
            <inherited>true</inherited>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-deploy-plugin</artifactId>  
            <configuration>  
              <updateReleaseInfo>true</updateReleaseInfo>  
            </configuration>  
          </plugin>  
        </plugins>  
      </build>  
    </profile>  
  </profiles>  
</project>
```
可以通过下面命令查看当前pom.xml受到超pom.xml文件的影响：
mvn help:effective-pom
c）聚合关系：用于将多个maven项目聚合为一个大的项目。
```html
<project xmlns="http://maven.apache.org/POM/4.0.0"  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0  
                      http://maven.apache.org/maven-v4_0_0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>org.codehaus.mojo</groupId>  
  <artifactId>my-parent</artifactId>  
  <version>2.0</version>  
  <modules>  
    <module>my-project<module>  
  </modules>  
</project>  
```

### 3、属性
maven的属性，是值的占位符，类似EL，类似ant的属性，比如${X}，可用于pom文件任何赋值的位置。有以下分类：
env.X：操作系统环境变量，比如${env.PATH}
project.x：pom文件中的属性，比如：<project><version>1.0</version></project>，引用方式：${project.version}
settings.x：settings.xml文件中的属性，比如：<settings><offline>false</offline></settings>，引用方式：${settings.offline}
Java System Properties：java.lang.System.getProperties()中的属性，比如java.home，引用方式：${java.home}
自定义：在pom文件中可以：<properties><installDir>c:/apps/cargo-installs</installDir></properties>，引用方式：${installDir}

### 4、构建设置
构建有两种build标签：
```html 
<project xmlns="http://maven.apache.org/POM/4.0.0"  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0  
                      http://maven.apache.org/maven-v4_0_0.xsd">  
  …  
  <!– "Project Build" contains more elements than just the BaseBuild set –>  
  <build>…</build>  
  <profiles>  
    <profile>  
      <!– "Profile Build" contains a subset of "Project Build"s elements –>  
      <build>…</build>  
    </profile>  
  </profiles>  
</project>
```
build中的主要标签：Resources和Plugins。

Resources：用于排除或包含某些资源文件

```html 
<resources>  
  <resource>  
    <targetPath>META-INF/plexus</targetPath>  
    <filtering>false</filtering>  
    <directory>${basedir}/src/main/plexus</directory>  
    <includes>  
      <include>configuration.xml</include>  
    </includes>  
    <excludes>  
      <exclude>**/*.properties</exclude>  
    </excludes>  
  </resource>  
</resources>
```
Plugins：设置构建的插件
```html 
<build>  
   …  
   <plugins>  
     <plugin>  
       <groupId>org.apache.maven.plugins</groupId>  
       <artifactId>maven-jar-plugin</artifactId>  
       <version>2.0</version>  
       <extensions>false</extensions>  
       <inherited>true</inherited>  
       <configuration>  
         <classifier>test</classifier>  
       </configuration>  
       <dependencies>…</dependencies>  
       <executions>…</executions>  
     </plugin>  
```
