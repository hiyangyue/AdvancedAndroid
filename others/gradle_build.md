
# Gralde : 构建
Gradle时基于Groovy语言，面向Java应用为主的一种项目构建工具。Android Studio默认使用Gradle工具，所以当创建一个新的project时，它的结构是这样子的 :


    rootProject/
        ├── gradlew
        ├── gradlew.bat
        └── gradle/wrapper/
        ├── gradle-wrapper.jar
        └── gradle-wrapper.properties
    

### Gradle Build命令

- assemble : 针对某个版本创建一个apk
- clean : 删除所有的构建任务(包含已经构建号的apk文件)
- check : 执行Lint检查，如果Lint检查到错误，则停止执行脚本
- build : 同时执行assemble，check命令
- connectedCheck : 执行测试任务
- deviceCheck : 执行在远程设备上的测试
- installDebug : 安装debug版本
- installRelease : 安装release版本
- uninstallDebug : 卸载debug版本
- uninstallRelease : 卸载release版本
- tasks :查看当前可构建的所有task
- -v : 查看当前的gradle版本

使用方法 ： gradlew + 命令, 比如创建一个debug版本的apk

    gradlew assembleDebug
    
 
 ### 构建带签名版本
 
 #### 生成keystore文件
 在命令行输入
 
    keytool -genkey -v -keystore myreleasekey.keystore
    
 根据提示设置相应的信息,即可

或者一次点击Build -> Generate Signed APK，根据提示输入相应的信息即可。

签名生成之后，在对应module下的build.gradle文件下

    
    android {
        ...
        defaultConfig { ... }
        signingConfigs {
            release {
                storeFile file("myreleasekey.keystore")
                storePassword "password"
                keyAlias "MyReleaseKey"
                keyPassword "password"
            }
        }
    
        buildTypes {
            release {
                ...
                signingConfig signingConfigs.release
            }
        }
    }

---
参考
 - [Signing Your Apps](http://developer.android.com/tools/publishing/app-signing.html)



