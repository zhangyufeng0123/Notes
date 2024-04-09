1. 首先得理解什么是静态，什么是动态

2. 架构：

   CS：Client Server

   CS不足：

   ​	a. 如果 软件升级，那么全部软件都需要升级

   ​	b. 维护麻烦：需要维护每一台客户端软件

   ​	c. 每一台客户端都需要安装客户端软件

   BS：Browser server

   ​	客户端可以通过浏览器直接访问服务端

3. tomcat解压后目录

   bin：可执行文件

   conf：配置文件

   lib：tomcat依赖的jar文件

   log：日志文件

   temp：临时文件

   webapps：可执行的项目

   work：存放由jsp翻译成的java，以及编译的class文件

4. 配置tomcat

   a. 配置jdk（必须配置JAVA_HOME)