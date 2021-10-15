第一天：
学生了解项目需求，整体架构及项目演示，要求作息时间，及制定纸条安排任务。
爱旅行的16张表：
爱旅行的各种开发规范：
        Swagger文档生成的使用，及文档规范。
        Postman测试接口/或者swagger自带的测试。
        演示参数的调用。
第二天：
Mavn ,nexus  swagger ,及代码生成器。
Nexus的搭建
上传jar包并解压，tar –xvf  bundle
修改端口号并开启
iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
启动
/opt/software/nexus-2.12.0-01/bin/nexus start
远程登录
http://服务器IP:8081/nexus/
开启主仓库索引
因为下载的很慢所以要替换掉。
重启
rm  –r * 删除所有文件
Java  -jar 包名
如果提示root 权限问题 ，那么执行export RUN_AS_USER=root临时操作，
或者在/etc/profile 环境变量配置中加入export RUN_AS_USER=root，需要执行source /etc/profile
默认的用户名和密码:admin  admin123
默认的库：
    Public 共有的库，所有的其他库的资源，都会更新到这里。
    3RD:  存放第三方的jar 包。
    Apache:关于Apache的包。
    Center:自己公司的所有的jar 包。
    Snapshots:快照版本
    Releases:发布版本。

本地项目配置并传到私服

<distributionManagement>
   <repository>
         <id>releases</id>
         <url>http://192.168.58.137:8081/nexus/content/repositories/releases/</url>
   </repository>
  <snapshotRepository>
       <id>snapshots</id>
       <url>http://192.168.58.137:8081/nexus/content/repositories/snapshots/</url>
  </snapshotRepository>
</distributionManagement>

配置文件
      <mirror>
			<id>alimaven</id>
			<name>aliyun maven</name>
			<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
			<mirrorOf>central</mirrorOf>
		</mirror>
		
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>*</mirrorOf>
      <name></name>
      <url>http://192.168.58.137:8081/nexus/content/groups/public/</url>
</mirror>
  <server>
      <id>snapshots</id>
      <username>admin</username>
      <password>admin123</password>
    </server>
Swagger的使用
1．引入jar包
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <junit.version>4.12</junit.version>
    <sources.plugin.verion>2.4</sources.plugin.verion>
    <cas.client.core.version>3.4.1</cas.client.core.version>
    <version.jackson>2.4.4</version.jackson>
    <jackson.verson>2.8.7</jackson.verson>
</properties>

<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.4.0</version>
    <exclusions>
        <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
        </exclusion>
        <exclusion>
            <groupId>com.fasterxml</groupId>
            <artifactId>classmate</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.4.0</version>
    <exclusions>
        <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>19.0</version>
</dependency>
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct-jdk8</artifactId>
    <version>1.1.0.Final</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>${jackson.verson}</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>${jackson.verson}</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>${jackson.verson}</version>
</dependency>


2.添加配置文件
 
3.配置静态文件扫描
 <mvc:default-servlet-handler />
4.在类上配置注解
@RequestMapping("/api")
@Api(value = "appinfo",description = "用户模块")
在方法上面添加
@ApiImplicitParams({
        @ApiImplicitParam(paramType="query",required=true,value="用户名",name="name",defaultValue="itrip@163.com"),
        @ApiImplicitParam(paramType="query",required=true,value="密码",name="password",defaultValue="111111"),
})
5paramType的值为
5.1.form
   Post请求
5.2. header
   @RequestHeader  映射参数时
5.3. query
    get请求：
5.4. path
    Rest方式
@ApiOperation(value = "测试",notes = "标题")
@ApiImplicitParam(paramType="path",required=true,value="用户名",name="id",defaultValue="1")

@RequestMapping(value = "/list_{id}",method =RequestMethod.GET )
@ResponseBody
public Object getid(@PathVariable("id")String id)
{
    System.out.println(id);
    return JSONArray.toJSONString("字符串");
}
5.启动tomcat 并访问
http://localhost:8080/项目路径/swagger-ui.html
freemarker
引入依赖
<dependency>
    <groupId>org.freemarker</groupId>
    <artifactId>freemarker</artifactId>
    <version>2.3.23</version>
</dependency>
创建测试类
 
编写模板
 
短信发送 荣联
https://www.yuntongxun.com/?ly=sem-baidu&qd=pc-dasou&cp=ppc&xl=null&kw=10233146&bd_vid=11068815637338301099

redis 的使用
redis安装
启动
解压tar包,然后进入目录
Make 进行编译
然后进入redis跟路径下
【vi redis.conf】
修改
把protected-mode yes改为protected-mode no（在没有密码的情况下，关闭保护模式）
注释掉bind 127.0.0.1     （取消绑定本地地址）
把daemonize no改为daemonize yes   （是否为进程守护，关闭ssh窗口后即是否在后台继
设置密码requirpass  123456  必须添加
【./redis-server】启动
开放端口，然后连接
查看ps aux|grep redis  
Kill -9 进程 可以杀死

守护模式启动
src/redis-server /opt/software/redis-5.0.2/redis.conf
启动和关闭
1.启动：redis-server（redis-server redis.conf）
2.登陆：redis-cli（redis-cli -p 6379）
3.关闭：redis-cli shutdown
关闭防火墙
systemctl stop firewalld.service
远程连接
Redis的数据类型
1.	String Key/value  key值唯一，如果重复替换value。
2.	List  Key/集合。
3.	Set  key/value  key可以重复，但是value必须唯一。
4.	Zset name  key/value  key+value 不能重复。
5.	Hash  Key/value       key与value 不能重复。

jedis连接Redis
1.引入包
  <dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.1.0</version>
</dependency>
2.加入帮助类RedisAPI
3.使用spring 注入配置

Ngnix的搭建
	安装配置步骤
	安装模块依赖库
	pcre库（rewrite）
	yum install pcre*
	pcre-8.32.tar.gz （下载地址： http://www.pcre.org/）
	openssl库（ssl）
	yum install openssl*
	openssl-fips-2.0.16.tar.gz （下载地址： http://www.openssl.org/）
	zlib库（gzip）
	yum install zlib*
	zlib-1.2.11.tar.gz（下载地址：http://www.zlib.net/）
	安装Nginx
	./configure  为当前的服务设置路径/usr/local
	make&make install
	开放80端口
iptables -I INPUT -p tcp --dport 80 -j ACCEPT

	启动Nginx
	usr/local/nginx/sbin/nginx
	访问Nginx
	http://服务器IP
	Nginx常用命令
	启动：usr/local/nginx/sbin/nginx
	停止：usr/local/nginx/sbin/nginx -s stop
	重启：/usr/local/nginx/sbin/nginx –s reload
	检查配置文件(nginx.conf)是否合法： /usr/local/nginx/sbin/nginx –t
查看文件路径：find /|grep nginx.conf
1.	ngix 的日志处理。
日志文件位置:/usr/local/nginx/logs 目录下
配置文件位置: /usr/local/nginx/conf  目录下的ngnix.conf
   配置自己的日志
放开:http 下的main
                    开启service 下的access_log
                    如果没有日志文件需要重新创建
                    注意重新启动服务器
2.	nginx 的反向代理。
     配置:http下面
   upstream my1{
        server 192.168.58.153:8080;
}
Server 下配置
 listen       70;
server_name  localhost;
location / {
               proxy_pass http://my1        
          }
如果想获取到真是的客户端IP客户端
在location 中添加
{
    #设置客户端真实ip地址
            #proxy_set_header X-real-ip $remote_addr;		
}

3.	ngix 实现负载均衡。
4.	正则表达式
location ~tent{
            return 401;
       }

        error_page  401              /401.html;
~ 为区分大小写匹配
 ~* 为不区分大小写匹配
location ~* .(gif|jpg|jpeg)$  
{expires 30d;
 }
# 匹配任何已.gif、.jpg 或 .jpeg 结尾的请求，但是 所有  /images/开
Windows下使用ngnix
在nginx.exe目录，打开命令行工具，用命令 启动/关闭/重启nginx 
 
start nginx : 启动nginx
nginx -s reload  ：修改配置后重新加载生效
nginx -s reopen  ：重新打开日志文件
nginx -t -c /path/to/nginx.conf 测试nginx配置文件是否正确

关闭nginx：
nginx -s stop  :快速停止nginx
nginx -s quit  ：完整有序的停止nginx

nginx -t -c D:\346-maven\nginx-1.16.1\conf\nginx.conf


如果遇到报错：
bash: nginx: command not found
有可能是你再linux命令行环境下运行了windows命令，
如果你之前是允许 nginx -s reload报错， 试下 ./nginx -s reload
或者 用windows系统自带命令行工具运行
Solr
界面操作
q条件匹配:*:*
Fq条件赛选id:1 OR id:2
Sort排序:id desc
Fl返回查询的字段

多关键字组合
在schema.xml中配置
<field name="keyword" type="string" indexed="true" stored="true" multiValued="true"/>

<copyField source="hotelName" dest="keyword" maxChars="3000"/>
<copyField source="address" dest="keyword" maxChars="3000"/>
重启tomcat 
重新导入数据
程序调用
添加依赖
<dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.25</version>
        </dependency>

        <dependency>
            <groupId>org.apache.solr</groupId>
            <artifactId>solr-solrj</artifactId>
            <version>5.3.1</version>
        </dependency>

        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>
添加实体类，并加入solr注解
@Field 
private Integer id;

编写测试代码
//初始化HttpSolrClient
HttpSolrClient httpSolrClient = new HttpSolrClient(url);
httpSolrClient.setParser(new XMLResponseParser()); // 设置响应解析器
httpSolrClient.setConnectionTimeout(500); // 建立连接的最长时间
//初始化SolrQuery
SolrQuery query = new SolrQuery("*:*");
query.setSort("id", SolrQuery.ORDER.desc);
query.setStart(0);
//一页显示多少条
query.setRows(10);
QueryResponse queryResponse = null;
try {
    queryResponse = httpSolrClient.query(query);
    List<Hotel> list = queryResponse.getBeans(Hotel.class);
    for (Hotel hotel:list){
        System.out.println(hotel.getAddress());
    }
} catch (SolrServerException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}

 

