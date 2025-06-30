---
title: mac系统下配置flutter环境
date: 2020-07-14 09:11
tags:
  - 博客
  - macos
  - flutter
categories:
  - 博客
  - macos
---

## 一、下载安装及配置

### 1、安装 Flutter SDK

进入 [Flutter官网](https://developer.android.google.cn/studio?hl=zh-cn) 下载 `Flutter SDK`

我安装时的下载地址：[https://flutter.dev/docs/get-started/install/macos](https://flutter.dev/docs/get-started/install/macos)

下载好后，将`flutter_macos_1.17.5-stable.zip`安装包，解压到不会轻易误删的地方。这里我解压到文稿中。也就是路径为/Users/你的mac名/Documents的位置。

编辑环境变量，打开 `bash_profile` 文件

```bash
open ~/.bash_pofile
```

在最后面插入下列代码

```bash
export PATH=你文件所在目录的路径/flutter/bin:$PATH
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
export PUB_HOSTED_URL=https://pub.flutter-io.cn
```

接着保存退出后，在终端输入 `source ~/.bash_profile` 即可使用flutter

```bash
flutter doctor
```

上面代码用于检测你还有哪些部分没有配置完成。

> 如果你终端是zsh，你还需要多一步

```zsh
open ~/.zshrc
```

在最下面添加这一行代码

```zsh
source ~/.bash_profile
```

### 2、安装 Xcode

直接在AppSotre中，搜索Xcode进行安装即可。

终端输入 `flutter doctor` 进行检查。

### 3、安装 Android Studio

进入 [Android Studio官网](https://developer.android.google.cn/studio?hl=zh-cn) 下载

下载后，按自己需要进行设置及安装。

安装结束后添加环境变量

```bash
open ~/.bash_profile
```

在最后一行添加下面代码

```bash
export PATH=${PATH}:~/Library/Android/sdk/platform-tools
```

```bash
source ~/.bash_profile
```

接着打开 Android Studio，在 Plugin 中搜索 `flutter` 并安装。它会自动安装 flutter、dart两个插件。

安装结束后可以输入 `flutter doctor` 检测是否成功。

### 4、下载 VScode

进入 [VScode官网](https://code.visualstudio.com/) 下载

下载好后添加 flutter、Dart 两个插件

终端输入 `flutter doctor` 进行检测。

### 5、安装夜神模拟器

进入 [夜神模拟器官网](https://www.yeshen.com/) 下载

安装并打开。

## 二、运行项目

在运行时为了增加流畅性，建议修改Flutter的配置

```bash
open 你的flutter文件目录/packages/flutter_tools/gradle/flutter.gradle
```

将repositories中的 `google()` 和 `jcenter()` 删除。替换为

```gradle
maven { url ‘https://maven.aliyun.com/repository/google’ }
maven { url ‘https://maven.aliyun.com/repository/jcenter’ }
maven { url ‘http://maven.aliyun.com/nexus/content/groups/public’ }
```

创建你的第一个项目

```bash
flutter create 项目名
cd 项目名
```

并修改你项目中的 `android/build.gradle` 的配置文件。同样修改 repositories 中的内容。只不过他有两个位置都要替换。

接着将项目与夜神模拟器建立连接，在终端输入

```bash
adb connect 127.0.0.1:62001
```

输入完没报错就是连接成功了。

> 如果报错，那是因为你夜神模拟器没有打开，要在打开的状态下在输入指令连接。

确认无误后，即可开始编译运行。

```bash
flutter run
```

