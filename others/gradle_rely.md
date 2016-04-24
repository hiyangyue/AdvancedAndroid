# Gradle : 依赖管理、其他设置
    
### 添加远程库
直接在build中的dependencies中添加依赖库


    dependencies {
        compile 'com.google.code.gson:gson:2.2.1'
    }
 
### 添加本地库
在setting.gradle文件中加入 

    include ':app' , ':volley'
   
 然后回到app目录下的build.gradle，在dependencies中添加 ： 
 
    dependenciess {
        compile project(' :volley')
    }
    
### 添加jar包

有两种方法
 1. 右键jar文件，选择"add as a library"
 2. 在gradle中添加
 

    compile fileTree(include: ['*.jar'], dir: 'libs')


### 添加aar本地库

    allprojects {
        repositories {
            jcenter()
            flatDir {
                dirs 'libs'
            }
        }
    }

    dependencies {
        compile(name:'本地库aar的名字', ext:'aar')
    }



#### 设置支持的语言

    android {
        defaultConfig {
            resConfigs "zh"
        }
    }

#### Lint选项开关
Lint是一套静态代码分析工具，它可以帮助我们检查项目中存在的问题，让我们开发更具有规范性的APP

    android{
        lintOptions {
            abortOnError false
        }
    }


#### 清除无用资源
编译APK时，没有不使用过的资源加载。

    android {
        buildTypes {
            release {
                minifyEnabled true
                shrinkResources true
            }
        }
    }

---
参考 ：
- [New Build System](http://tools.android.com/tech-docs/new-build-system)
