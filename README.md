## 创建Config-server和Config-client

** 第一部分 - 创建Conifg-server

1. 创建Spring-boot应用
   * 使用start.spring.io(http://start.spring.io/) 或者IDE创建应用
   * 命名为cofig-server 
   * 设置端口为8888

2. 检查依赖
    * spring-cloud-config-server

3. 使用@EnableConfigServer定义SpringBoot的主类


4. 创建配置文件相关目录config-repo
    * 本地文件
    * 或者github


5. 在config-repo中定义配置文件，如"{spring-application}-{profile}.yml” (或者properties文件).
    * 本例中使用Git和本地文件，并定义event-service.properties
    * 定义database.username=admin
    * 定义分支testdatabase.username=[test]admin    


6. 运行config-server，访问http://localhost:8888/event-service/default/
   或者尝试访问如下不同的URL
    /{application}/{profile}
    http://localhost:8888/event-service/development
    //event-service-development.properties
    http://localhost:8888/event-service/production
    //event-service-production.properties

    /{application}/{profile}[/{label}]
    http://localhost:8888/event-service/development/master
    //event-service-development.properties in master branch

    http://localhost:8888/event-service/production/test
    //event-service-production.properties in test branch

------------------------------------------------------------------------------------------

** 第二部分 - 创建Conifg-client

9. Create a new, separate Spring Boot application. 
Name the project "event-service", and use this value for the Artifact.  
Add the web dependency.  
Use Jar packaging and the latest versions of Java.  
	* access http://start.spring.io/ or any IDE, create a new project
	* Open ConfigServerApplication and add annotation @EnableConfigServer


10. Add a bootstrap.yml (or bootstrap.properties) file in the root of your classpath (src/main/resources recommended).  Add the following key/values using the appropriate format:
spring.application.name=event-service
server.port=8001
spring.cloud.config.uri=http://localhost:8888

_(Note that this file must be "boostrap" -- not "application" -- so that it is read early in the application startup process.  The server.port could be specified in either file, but the URI to the config server affects the startup sequence.)_

11. Add a REST controller to obtain and display the db relevant configurations:

12.  Start your client.  Open [http://localhost:9999](http://localhost:9999).  
You should see the config message in your browser.

------------------------------------------------------------------------------------------

** Part 3 - Access configuration file by Spring profile

13. Create a separate file in your GitHub repository called "event-service-production.properties” (or .yml).  
Populate it with the "db*" key and a different value than used in the original file.

14. Stop the client application.  Modify the boostrap file to contain a key of spring.profiles.active: production.  Save, and restart your client.  
Access the URL.  Which db* is displayed?  (You could also run the application with -Dspring.profiles.active=production rather than changing the bootstrap file)


------------------------------------------------------------------------------------------

** Part 4 - Access configuration file by automatically refresh
15. Add Spring Boot Actuator 
16. Return to your EventController and convert to @RefreshScope
17. Change the configuration in event-service.properties, for instance "database.password=password3333"
18. execute 'curl -X POST http://localhost:9999/refresh', you will get the result includes '["database.password"]' in console to represent the database.password has been changed
19. then, refresh the page 'http://localhost:9999', you will get the new password value in browser.

------------------------------------------------------------------------------------------




  