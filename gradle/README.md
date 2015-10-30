
#gradle 学习

Gradle 基本概念
首先我们学习几个gradle 的脚步语法，掌握了这几个语法，你就能非常简单的用gradle构建打包android项目了。 首先，我们来看下一个最简单android build.gradle。
build.gradle
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
	
buildscript {
       
	 repositories {
            mavenCentral()
        }

        dependencies {
            classpath 'com.android.tools.build:gradle:0.4'
        }
    }



参考文档：
		最权威的官方打包指南（需要翻墙）http://tools.android.com/tech-docs/new-build-system
		Android打包插件API（在线版）http://apdr.qiniudn.com/index.html 

如果你对于手动配置build.gradle文件还不太熟练，那么可以使用AS提供的图形界面来配置，按下CMD+;即可打开配置页面


新特性：
     Google在用Gradle最为Android打包工具的时候引入了applicationId的概念，这是为了打多个不同ID的APK包准备的。
applicationId可以和清单文件中的packageName不一样，我们在代码中通过getPackageName()方法拿到的是applicationId，而清单文件中配置的packageName则仅作为R.java和BuildConfig.java的存放目录。
这样一来通过Class.forName(getPackageName()+”.R”)来获取R类的方式就行不通了，一定要注意。

打包

build.gradle文件配置完成后，打开终端，进入项目目录下，执行gradle build即可打包，打包结束后在相应module的build/outputs/apk/目录下可以看到.apk文件

如果你是在项目目录下执行的打包命令，那么会对项目中所有的module都打包，进入某个module目录下执行打包命令就只对当前module打包，每个module打包生成的APK都才存放在mudule的build/outputs/apk目录下





