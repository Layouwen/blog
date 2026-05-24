---
title: Tomcat 篇
date: 2024-01-23T15:28:00.000Z
tags:
  - 汇总
  - tomcat
categories:
  - 汇总
  - 开发环境
uuid: d66ada57-ca31-4b8a-a719-f9e37295eb87
---

# 请求处理

在java中创建class
**java/web/TestServlet.class**
```java
package web;

...

public class TestServlet extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    System.out.println("get");
  }

  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    System.out.println("post");
  }
}
```

**web.xml**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>test</servlet-name>
        <servlet-class>web.TestServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>test</servlet-name>
        <url-pattern>/test</url-pattern>
    </servlet-mapping>
</web-app>
```

访问 [http://localhost:8080/test](http://localhost:8080/test)

通过注解方式

```java
@WebServlet(urlPatterns = "/user", name = "user", initParams = {
  @WebInitParam(name = "name", value = "wangyu")
})
public class UserServlet extends HttpServlet {
  @Override
  public void init(ServletConfig config) throws ServletException {
    System.out.println(config.getInitParameter("name"));
  }

  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    System.out.println("get");
  }
}

```

# 初始化配置

**web.xml**
```xml
    <!-- 全局配置 -->
    <context-param>
        <param-name>age</param-name>
        <param-value>21</param-value>
    </context-param>
    <servlet>
        <servlet-name>test</servlet-name>
        <servlet-class>web.TestServlet</servlet-class>
        <!-- 局部配置 -->
        <init-param>
            <param-name>name</param-name>
            <param-value>layouwen</param-value>
        </init-param>
    </servlet>
```

# 常见配置

## 修改端口号

修改 port 的值即可

**conf/server.xml**
```xml
<Connector port="9999" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
```

# JSP内置对象
