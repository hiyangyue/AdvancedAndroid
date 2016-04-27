# Gradle : 多版本 && 差异化

- buildTypes 
    
    可以为不同的用户、不同的渠道打包不同的 apk 包，以便在程序行为上针对不同使用需求做少量定制（如是否可以更改远程服务器地址等)
   
- productFlavors

    productFlavors主要针对于"不同的版本之间差异非常小，本质上是同一应用"这种情况。可以根据渠道不同在版本名中增加相应后缀， 可以根据特殊客户要求打包不同 ApplicationID 的 apk 包，并可分别使用不同的资源文件（如不同的应用图标等）

#### 创建自己的构建版本
在buildTypes中添加，如下 ： 

    android {
        ...
        buildTypes {
            release {
                ...
            }
            
            debug {
                ...
            }
            
            //自定义的版本
            baidu {
                minifyEnabled false
                proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
                applicationId "com.xxxx.test.baidu"
            }
            
            wandoujia.initWith(buildTypes.release)
            wandoujia {
                applicationId "com.xxxx.test.wandoujia"
            }
        }
    }
    
 initWith可以理解为buildTypes中的继承，如果已经存在对应的设置，使用initWith是最简洁的方式。
 
 #### 添加单独的依赖包
 默认情况添加依赖包是应用所有的版本，如果想要为某个版本单独设置，比如为debug版本单独添加一个Gson的依赖包，则可以写成 ：
 
    dependencies {
        compile 'com.android.support:appcompat-v7:22.2.0'
        debugCompile 'com.google.code.gson:gson:2.2.1'
    }
    

#### product flavor的 创建
在build.gradle中声明相应的版本即可，这里创建里两个flavor : pro, free。

    android {
        ...
        
        productFlavors {
            pro {
                
            }
            
            free {
                
            }
        }
    }

#### product flavor的设置
和buildTypes类似： 

    productFlavors {
        pro {
            packageName "com.hiyangyue.test.pro"
            versionCode 3
        }
        
        free {
            packageName "com.hiyangyue.test.free"
            versionCode 10
            miniSdkVersion 14
        }
    }

---
参考 ：
 - [Configuring Gradle Builds](http://developer.android.com/tools/building/configuring-gradle.html)
 - [构建变种版本 - Build Variants](http://wiki.jikexueyuan.com/project/android-gradle-guide/build-variants.html)
 - [Gradle Plugin User Guide](http://tools.android.com/tech-docs/new-build-system/user-guide)
 - [利用 Android Studio 和 Gradle 打包多版本APK](http://chobitly.github.io/2016/02/05/Android-Gradle-Build/)
 - [Gradle for Android 第四篇( 构建变体 )](https://segmentfault.com/a/1190000004241503#articleHeader2)