#javaBug

##1.Failed to execute goal org.apache.tomcat.maven:tomcat7-maven-plugin:2.2:run (default-cli) on project
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.2.1-b03</version>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>3.0-alpha-1</version>
</dependency>
项目继承了Spring MVC框架，对jsp页面处理依赖下面两个jav包，启动时候就报错了，但是这两个包和tomcat插件中的包冲突导致，因此应该加上限定<scope>provided</scope>,但是返回视图时，又出现了新的错误（  java.lang.ClassNotFoundException: （javax.servlet.jsp.jstl.core.Config），少了依赖包：
<dependency>
    <groupId>javax.servlet.jsp.jstl</groupId>
    <artifactId>jstl-api</artifactId>
    <version>1.2-rev-1</version>
</dependency>

 启动时又出现之前的错误：

[ERROR] Failed to execute goal org.apache.tomcat.maven:tomcat7-maven-plugin:2.2:run (default-cli) on project configuration: Could not start Tomcat: Failed to start component [StandardServer[-1]]: Failed to start component [StandardService[Tomcat]]: Failed to start component [StandardEngine[Tomcat]]: A child container failed during start -> [Help 1]

    试着运行一下war包，没有问题，难道又是scope的问题，加上provided无效，改成runtime也无效。难道是servlet版本（我用的是2.5）的问题吗？

    切换servlet版本后依然报同样的错。

    莫非是jstl版本的问题，是不是版本高了？

    试着把1.2后面的内容去掉，一切OK！

    <dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.2.1-b03</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>3.0-alpha-1</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>javax.servlet.jsp.jstl</groupId>
    <artifactId>jstl-api</artifactId>
    <version>1.2</version>
</dependency>

##2. Failed to execute goal org.springframework.boot:spring-boot-maven-plugin:1.5.2.RELEASE:run (default-cli) on project
是因为8080端口被占用了，lsof -i -P | grep -i "listen",kill -9 pid ,mvn spring-boot run


##3.No plugin found for prefix 'tomcat7' in the current project and in the plugin groups [org.apache.maven.plugins
解决办法：

    找到maven 的setting.xml 在<pluginDroups>处添加以下信息

<pluginGroups>
  <pluginGroup>org.apache.tomcat.maven</pluginGroup>
</pluginGroups>