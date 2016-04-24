# Android Studio编译加速

#### 并行编译守护进程

  全局设置 ：在./../gradle中创建 gradle.propertites

    org.gradle.daemon=true
    org.gradle.jvmargs=-Xmx2048m -XX:MaxPermSize=512m 
    -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
    org.gradle.parallel=true
    org.gradle.configureondemand=true

  (如果项目设置 ，修改项目下的gradle.propertites文件即可)


#### 扩大运行内存
在Android Studio的安装目 : ...\AndroidStudio\bin，有两个文件

studio.exe.vmoptions

studio64.exe.vmoptions,分别对应着32和64位，使用编译工具打开，修改编译内存
添加如下参数 :

    -Xms512m
    -Xmx2048m
    -XX:MaxPermSize=512m

    
 
#### 引用本地的aar
把aar文件放在指定的目录里，比如libs，接着在build.gradle中添加如下内容 ： 


    repositories {
        flatDir {
            dirs 'libs' //this way we can find the .aar file in libs folder
        }
    }

之后在gradle中引用即可


    dependencies {
        compile(name:'test', ext:'aar')
    }

#### 打开dex增量编译

    dexOptions {
        incremental true
    }
    

#### 使用固定的依赖库版本

  既然用到库，就会遇到库版本和升级id问题，使用自动升级最新的库有两个坏处
 1. 如果新的库接口改了，可能需要重新适配
 2. 如果当前的网络比较差，拉去最新的库则会影响编译的速度

其中maven库的依赖管理支持一种动态版本的方式。
如果依赖某个版本的库，可以写成 ：


    dependenies {
      compile 'com.google.code.gson:gson:2.2.1'
    }

如果不想依赖具体的库，想每次拉取最新的版本，则可以写成


    dependenies {
      compile 'com.google.code.gson:gson:2.＋'
    }


也可以写成

      dependenies {
        compile 'com.google.code.gson:gson:＋'
      }



#### 使用Offline Mode
每次启动都会有联网检测依赖的文件是否到了最新版。如果没有特殊需求，可以选择不缺更新，就可以在Android Studio中选的offline模式，这样就不会去更新了。


#### 开启Instant run

#### 使用最新版本的Gradle


---
参考 ：

- [GRADLE构建最佳实践](http://www.figotan.org/2016/04/01/gradle-on-android-best-practise/)
- [6 tips to speed up your Gradle build](https://medium.com/@shelajev/6-tips-to-speed-up-your-gradle-build-3d98791d3df9#.2n2lsqalt)
- [如何优化 Android Studio 启动、编译和运行速度？](https://segmentfault.com/a/1190000004044038)
- [Android模块化编程之引用本地的aar](http://stormzhang.com/android/2015/03/01/android-reference-local-aar/)