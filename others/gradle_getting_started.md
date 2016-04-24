# Gradle : build.gradle入门

### 
AS在创建新Project之后，会自动创建三个gradle文件，两个build.gradle，一个setting.gradle，结构图如下

    MyApp
      ├── build.gradle
      ├── settings.gradle
      └── app
           └── build.gradle
       
### setting.gradle 
settubg,gradle定义了哪个模块将会被构建，第一次构建的时候，project中只有一个app的module，所以它是这样子的 ：

    include : "app "

添加project中的其他module，只要在setting.gradle中加入module的名称

    include : "app" , "xxx"
 
 
### 根目录的build.gradle
默认情况下build.gralde如下 ： 

    buildscript {
        repositories {
            jcenter ()
        }
        
        dependencies {
            classpath 'com.android.tools.build:gradle:2.0'
        }
    }
    
    allprojects {
        repositories {
            jcenter ()
        }
    }

**buildscript** 定义了全局的相关属性，repositories定义了仓库源，默认jcenter() ，可以根据需求添加maven仓库等等。

**depedencies** 表示Android Studio Gradle的版本，一般不需要更改。

**allprojects** 用来定义不同module的属性，默认表示时 为每个module设置jcenter()仓库依赖

### module中的build.gradle
module中默认的build.gradle应该是这样子的 :

    apply plugin: 'com.android.application'

    android {
        compileSdkVersion 23            //用于编译的SDK版本
        buildToolsVersion "23.0.3"

        defaultConfig {
            applicationId "com.hiyangyue.test"   //app的程序包名
            minSdkVersion 19                     //最低支持Android版本
            targetSdkVersion 23                  //测试环境下的Android版本
            versionCode 1                        //版本号
            versionName "1.0"                    //版本名
        }
        
        buildTypes {
            release {
                minifyEnabled false             //是否启用混淆
                proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            }
        }
    }

    //用于配置的依赖
    dependencies {
        compile fileTree(dir: 'libs', include: ['*.jar'])
        testCompile 'junit:junit:4.12'
        compile 'com.android.support:appcompat-v7:23.3.0'
    }


### 设置全局变量
在Projects的build.gradle中定义全局变量

    ext {
        compileSdkVersion = "23"
        buildToolsVersion = "23.0.2"
        miniSdkVersion = "14"
        targetSdkVersion = "23"
    }
    
 module中引用
   
    android { 
        compileSdkVersion rootProject.ext.compileSdkVersion
        buildToolsVersion rootProject.ext.buildToolsVersion
      
      defaultConfig {
          minSdkVersion rootProject.ext.minSdkVersion
          targetSdkVersion rootProject.ext.targetSdkVersion
      }
    }


---
参考 
 - [Android Studio 常用功能介绍]http://ask.android-studio.org/?/article/23)
 - [Gradle for Android 第二篇( Build.gradle入门 )](https://segmentfault.com/a/1190000004234712)
