---
title: Mac上安装Appium环境
date: 2016-06-10 21:25:31
tags: appium
#categories: python
#toc: true
---
周末花了半天时间捣鼓Appium安装，虽然[Appium官网](http://appium.io)提供的步骤很简单，但是安装过程中还是踩了一些坑。所以有必要将我在Mac电脑上的安装步骤记录下来，帮助大家避免入坑。
<!--more-->
Mac平台能真正发挥Appium的功能，因为Mac平台既能测试Android App又能测试iOS App。公司恰好给配的Macbook Pro，简直幸运至极。
下面是整理的安装Appium的完整过程，包括Mac平台的环境安装、以及Appium的安装。
# 0、Mac平台基础环境
先保证Mac平台已经有了下面这些软件。再进行Appium的安装。

## 1.java
```
liuchunmings-MacBook-Pro:~ liuchunming$ java -version
java version "1.8.0_66"
Java(TM) SE Runtime Environment (build 1.8.0_66-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.66-b17, mixed mode)
```
## 2.Git
```
liuchunmings-MacBook-Pro:~ liuchunming$ git --version
git version 2.4.9 (Apple Git-60)
```
## 3.Ruby
```
liuchunmings-MacBook-Pro:~ liuchunming$ ruby -v
ruby 2.0.0p481 (2014-05-08 revision 45883) [universal.x86_64-darwin14]
```
## 4. brew
```
liuchunmings-MacBook-Pro:~ liuchunming$ brew -v
Homebrew 0.9.9 (git revision f1293; last commit 2016-05-30)
Homebrew/homebrew-core (git revision c7ac; last commit 2016-05-31)
```
brew是Mac OS不可或缺的套件管理器。安装方法是：
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
## 5.Xcode
测试iOS App需要。
打开Finder，在Applications文件夹下，看是否有Xcode.app程序。如果没有，则需要安装。
下载地址：https://developer.apple.com/downloads/
安装方法同所有的dmg包的安装方法一样。

## 6.Android SDK
测试Android App需要。
下载地址：https://developer.android.com/studio/index.html#downloads
选择：android-sdk_r24.4.1-macosx.zip（写本文时的最新版）解压缩到任意位置，比如/usr/local/android-sdk-macosx下。
运行/usr/local/android-sdk-macosx/tools/android，即可启动Android SDK Manager。如图1。
图1：
![这里写图片描述](http://img.blog.csdn.net/20160601102612456)
可以在这里下载和更新 Android SDK Tolls 和 Android SDK Platform-tools 。保持默认的选项即可，点击Install 23 packages...。进入到图2。
图2：
![这里写图片描述](http://img.blog.csdn.net/20160601102758911)
Accept License。然后Install就可以了。这个过程根据网速不同，可能需要10-20分钟，耐心等待。

## 7.设置环境变量
在~/.bash_profile中新加下面两行。之后执行：source ~/.bash_profile 使环境变量生效。
> export JAVA_HOME=$(/usr/libexec/java_home)
> export ANDROID_HOME=/usr/local/android-sdk-macosx

至此，为了安装Appium所需要的Mac平台已经配置完毕了。接下来开始安装Appium。

# 1、Appium安装
Mac平台环境安装完毕之后，就可以开始安装Appium了。
Mac下搭建appium环境有两种方法：
1.直接下载appium.dmg 运行即可
2.使用npm安装
下载dmg包安装的方法，很简单，和安装所有的dmg包一样。不多介绍了。下面主要介绍下通过npm安装的方法。
官网上提供的步骤是下面这样的：
```
brew install node      # get node.js
npm install -g appium  # get appium
npm install wd         # get appium client
appium &               # start appium
node your-appium-test.js
```
我也是按照这个步骤来进行的。

## 1. 安装node.js

Appium依赖Node.js环境，因此需要先安装node环境。安装方法是执行brew install node。
安装完成后，可以执行node -v查看node版本。
```
liuchunmings-MacBook-Pro:~ liuchunming$ node -v
v6.2.0
```
> 坑：
先升级homebrew：brew update，以便能够安装最新版的node。我第一遍安装的时候，就是因为没有升级brew，所以通过brew install node安装的node版本比较低，导致用npm安装appium提示“'appnium' is not in the npm registry.”

## 2.安装 appium server
在终端输入npm install -g appium。
这个过程可能会比较慢。

## 3.安装appium client
在终端输入npm install wd。

# 2、检查环境
appium doctor用来appium的是否成功安装。下载appium doctor的网址在：https://github.com/appium/appium-doctor
在终端执行npm install appium-doctor -g来安装doctor。
安装完成后，终端输入appium-doctor 检测环境是否成功。
```
liuchunmings-MacBook-Pro:tools liuchunming$ appium-doctor
```
输出信息像下面这样全是对号，则表示环境安装成功了。
>info AppiumDoctor ### Diagnostic starting ###
info AppiumDoctor  ✔ Xcode is installed at: /Applications/Xcode.app/Contents/Developer
info AppiumDoctor  ✔ Xcode Command Line Tools are installed.
info AppiumDoctor  ✔ DevToolsSecurity is enabled.
info AppiumDoctor  ✔ The Authorization DB is set up properly.
info AppiumDoctor  ✔ The Node.js binary was found at: /usr/local/bin/node
info AppiumDoctor  ✔ HOME is set to: /Users/liuchunming
info AppiumDoctor  ✔ ANDROID_HOME is set to: /usr/local/android-sdk-macosx
info AppiumDoctor  ✔ JAVA_HOME is set to: /Library/Java/JavaVirtualMachines/jdk1.8.0_66.jdk/Contents/Home
info AppiumDoctor  ✔ adb exists at: /usr/local/android-sdk-macosx/platform-tools/adb
info AppiumDoctor  ✔ android exists at: /usr/local/android-sdk-macosx/tools/android
info AppiumDoctor  ✔ emulator exists at: /usr/local/android-sdk-macosx/tools/emulator
info AppiumDoctor ### Diagnostic completed, no fix needed. ###
info AppiumDoctor 
info AppiumDoctor Everything looks good, bye!
info AppiumDoctor 

# 3、启动appium
在终端输入appium &。
```
liuchunmings-MacBook-Pro:~ liuchunming$ appium & 
```
输出下面的信息，则表示appium server启动成功了。
>[1] 12649
liuchunmings-MacBook-Pro:~ liuchunming$ [Appium] Welcome to Appium v1.5.2 (REV f12932cf3176ffea5f4004984a390e8dc929ebbf)
[Appium] Appium REST http interface listener started on 0.0.0.0:4723

至此，Appium就安装完毕了。
