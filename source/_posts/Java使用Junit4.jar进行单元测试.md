---
title: Java使用Junit4.jar进行单元测试
date: 2021-11-07 23:13
tags:
  - 博客
  - 后端
  - java
categories:
  - 博客
  - 后端
  - java
---

## 一、下载依赖包

分别下载 [junit.jar](https://repo1.maven.org/maven2/junit/junit/4.13.2/junit-4.13.2.jar) 以及 [hamcrest-core.jar](https://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar)

## 二、添加到依赖

在项目跟目录创建 `lib` 文件夹，并把下载的依赖包拖进去

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ac9ddc6fdc2044c390f30f8a1eeacdbd~tplv-k3u1fbpfcp-zoom-1.image](https://z3.ax1x.com/2021/11/07/I3NlTK.png)

鼠标移动到依赖包上，右键选择 `Add as Library...`。依次添加 `junit.jar` 以及 `hamcrest-core.jar` 到 library 中

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e209db9aa1e34923881791a9b08f5c95~tplv-k3u1fbpfcp-zoom-1.image](https://z3.ax1x.com/2021/11/07/I3Nclj.png)

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b0bcd4ad8f62479c8495366026b9d984~tplv-k3u1fbpfcp-zoom-1.image](https://z3.ax1x.com/2021/11/07/I3UknI.png)

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/74259d3917e24dd987897f8f4620810c~tplv-k3u1fbpfcp-zoom-1.image](https://z3.ax1x.com/2021/11/07/I3UEHP.png)

## 三、设置 test 目录

在项目根目录创建 `test` 文件夹，右键设置 `Mark Directory as` —— `Test Sources Root`

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0c8e70cfd49144d08cc8326bdd6439b4~tplv-k3u1fbpfcp-zoom-1.image](https://z3.ax1x.com/2021/11/07/I3afiT.png)

## 四、创建测试类

随便写一个类，用于演示

**HaHa.java**
```java
public class HaHa {
  public void sayHaHa() {
    System.out.println("HaHa");
  }

  public String sayBye() {
    System.out.println("Bye");
    return "Byt";
  }
}
```

`Ctrl + Shift + T` 快速创建 Test 代码。选择 `Create New Test...` 并选择你需要测试的方法。

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6391ce24cfe74db2a9a090c04edebe40~tplv-k3u1fbpfcp-zoom-1.image](https://z3.ax1x.com/2021/11/07/I3U5DI.png)

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d0b7b3139b724792908edb8abd38e0c4~tplv-k3u1fbpfcp-zoom-1.image](https://z3.ax1x.com/2021/11/07/I3apV0.png)

## 五、开始测试

**HaHaTest.java**
```java
import org.junit.Assert;
import org.junit.Test;

import static org.junit.Assert.*;

public class HaHaTest {

  @Test
  public void sayHaHa() {
    new HaHa().sayHaHa();
  }

  @Test
  public void sayBye() {
    String bye = new HaHa().sayBye();
    Assert.assertEquals("Byt", bye);
  }
}
```

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cb08824fe245433f8b0e5c471dd73a10~tplv-k3u1fbpfcp-zoom-1.image](https://z3.ax1x.com/2021/11/07/I3dpyd.png)