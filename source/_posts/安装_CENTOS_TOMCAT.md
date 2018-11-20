---
title: 安装_CENTOS_TOMCAT
date: 2018-11-19 18:29:49
tags: [安装,CENTOS]
---

# 1 下载 [tomcat](https://tomcat.apache.org)
```
cd /opt/software/packages
wget http://mirrors.hust.edu.cn/apache/tomcat/tomcat-8/v8.5.29/bin/apache-tomcat-8.5.29.tar.gz
tar -xzvf /opt/software/packages/apache-tomcat-8.5.29.tar.gz -C /opt/software/
```

# 2 启动 tomcat
```
/opt/software/apache-tomcat-8.5.29/bin/startup.sh
```
```
Using CATALINA_BASE:   /opt/software/apache-tomcat-8.5.29
Using CATALINA_HOME:   /opt/software/apache-tomcat-8.5.29
Using CATALINA_TMPDIR: /opt/software/apache-tomcat-8.5.29/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /opt/software/apache-tomcat-8.5.29/bin/bootstrap.jar:/opt/software/apache-tomcat-8.5.29/bin/tomcat-juli.jar
Tomcat started.
```

# 3 访问 tomcat
在浏览器中输入地址 http://10.10.10.10:8080 注意tomcat默认监听8080端口
![image.png](https://upload-images.jianshu.io/upload_images/2062311-09ee7dd56abb46e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 4 配置端口tomcat默认端口8080 为服务器默认的 80 端口
```
# 1024以下的端口只能由root用户使用，普通权限的tomcat服使用80端口启动时会报没有权限
# 以下方法为重定向 80 端口 到 8080, 避免了权限问题
iptables -t nat -A PREROUTING -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 8080  
iptables -t nat -A PREROUTING -p udp -m udp --dport 80 -j REDIRECT --to-ports 8080
service iptables save
service iptables restart
```
```
# 或者
vi /etc/sysconfig/iptables

-A PREROUTING -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 8080
-A PREROUTING -p udp -m udp --dport 80 -j REDIRECT --to-ports 8080

service iptables restart
```
```
在浏览器中访问 http://10.10.10.10:80
```

# 5 开机自启
```
chmod +x /etc/rc.d/rc.local
vi /etc/rc.d/rc.local
# 在最下面添加
/opt/software/apache-tomcat-8.5.29/bin/startup.sh
```

# 6 常用的防火墙配置
```
#iptables-save v1.4.7 on Wed Mar 21 13:50:00 2018
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 8000 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 7777 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 8888 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 8080 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 27017 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -p tcp -m tcp --dport 21 -m conntrack --ctstate NEW,ESTABLISHED -m comment --comment "Allow ftp connections on port 21" -j ACCEPT
-A INPUT -p tcp -m tcp --dport 20 -m conntrack --ctstate RELATED,ESTABLISHED -m comment --comment "Allow ftp connections on port 20" -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 10060:10090 -j ACCEPT
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
-A OUTPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A OUTPUT -p tcp -m tcp --dport 21 -m conntrack --ctstate NEW,ESTABLISHED -m comment --comment "Allow ftp connections on port 21" -j ACCEPT
-A OUTPUT -p tcp -m tcp --dport 20 -m conntrack --ctstate ESTABLISHED -m comment --comment "Allow ftp connections on port 20" -j ACCEPT
-A OUTPUT -p tcp -m state --state NEW -m tcp --sport 10060:10090 -j ACCEPT
-A OUTPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
COMMIT
# Completed on Wed Mar 21 13:50:00 2018

*nat
:PREROUTING ACCEPT [0:0]
:POSTROUTING ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A PREROUTING -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 8080
-A PREROUTING -p udp -m udp --dport 80 -j REDIRECT --to-ports 8080
COMMIT
```

# 7 tomcat webapps 目录启用软连接文件夹
* 修改 conf/context.xml
```
<Context>

    <!-- Default set of monitored resources. If one of these changes, the    -->
    <!-- web application will be reloaded.                                   -->
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
    <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>

    <!-- Uncomment this to disable session persistence across Tomcat restarts -->
    <!--
    <Manager pathname="" />
    -->
    <Resources allowLinking="true"/> # 加上这句话即可, 注:java 对文件夹软链接的访问使用 'dir.canonicalFile'
</Context>
```

# 8 tomcat 指定默认的启动工程
* 修改 /conf/server.xml
1. 当appBase采用相对路径时，其父路径是tomcat的安装目录。即：/opt/software/apache-tomcat-8.5.29
如：appBase = "/webapps" 或 appBase = “webapps” 相当于 appBase = “/opt/software/apache-tomcat-8.5.29/webapps/”
2. 当appBase使用绝对路径时，显然不需要注意什么，这也是推荐的写法。
3. 当docBase采用相对路径时，其父路径是appBase的路径。即：/opt/software/apache-tomcat-8.5.29
如：docBase = “/template”或docBase = “template”相当于 docBase = “/opt/software/apache-tomcat-8.5.29/template”
4. 特别的，路径显然只能指向某一文件夹，而不能具体的指向某个文件或是图片资源什么的。
```
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

        # 加上这句话即可
        <Context path="" reloadable="true" debug="5" docBase="template" /> 

        <!-- SingleSignOn valve, share authentication between web applications
             Documentation at: /docs/config/valve.html -->
        <!--
        <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
        -->

        <!-- Access log processes all example.
             Documentation at: /docs/config/valve.html
             Note: The pattern used is equivalent to using pattern="common" -->
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />

      </Host>
```

#9 由 tomcat 导致的中文乱码问题
* 首先确保 html 文件本身是 UTF-8 编码, 并且在 html 代码中设置编码格式
```
<meta http-equiv="Content-Type" content="text/html;charset=utf-8">
```
* 测试本地正常, 但是经过tomcat 中转后出现中文乱码, 尤其是纯英文的操作系统, 部分电脑正常部分电脑乱码, 有的电脑谷歌浏览器乱码但是火狐浏览器正常, 针对谷歌浏览器有一个临时解决方案,将 **设置->高级->语言->中文（简体)** 排序到最顶部 可以临时解决乱码
* 修改 /conf/server.xml
```
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443"
               URIEncoding="UTF-8" />
```

* 如果上述修改不起作用, 则需要在 spring boot 项目中增加静态资源过滤器,强制设置静态资源编码为 UTF-8
```
package com.smart.template.base.config.filter

import org.slf4j.LoggerFactory
import org.springframework.core.annotation.Order
import org.springframework.web.servlet.resource.ResourceUrlEncodingFilter
import springfox.documentation.annotations.ApiIgnore
import javax.servlet.FilterChain
import javax.servlet.ServletRequest
import javax.servlet.ServletResponse
import javax.servlet.annotation.WebFilter
import javax.servlet.http.HttpServletRequest

@ApiIgnore
@Order(1)
@WebFilter(filterName = "static-filter", urlPatterns = arrayOf("/*")) //拦截器拦截的是域名+端口相同的路径
class CXResourceUrlFilter : ResourceUrlEncodingFilter() {

    private val log = LoggerFactory.getLogger(this.javaClass)

    override fun doFilter(request: ServletRequest?, response: ServletResponse?, filterChain: FilterChain?) {
        val httpRequest = request as HttpServletRequest
        log.error("---------->doFilter start(${response?.contentType}) " + httpRequest.requestURI)
        response?.contentType = "UTF-8"

        super.doFilter(request, response, filterChain)
        log.error("---------->doFilter end(${response?.contentType}) " + httpRequest.requestURI)
    }
}
```
* 虽然设置过滤器解决了乱码问题, 比如通过访问 [http://10.47.12.111/index.html](http://10.47.12.111/index.html) 已经显示正常, 但是如果是项目的默认首页, 比如通过访问  [http://10.47.12.111](http://10.47.12.111) 就可以看到 
 index.html  所展示的页面*(只要将 index.html 放到 src/main/resources/public/ 目录下即可实现在 url 中不加 **/index.html** 也可访问到页面)*, 由于 url  [http://10.47.12.111](http://10.47.12.111)  中没有path 所以无法被上一步骤的拦截器所拦截, 此时还有一个冷门却有效的做法
```
<mime-mapping> 
       <extension>html</extension> 
       <mime-type>text/html;charset=utf-8</mime-type> 
</mime-mapping> 
```


