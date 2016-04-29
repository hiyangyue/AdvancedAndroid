# Proguard代码混淆

 ProGuard是一个免费Java类文件收缩，优化，混淆和预校验器。它可以检测并删除未使用的类，字段，方法和属性。它可以优化字节码，并删除未使用的指令。它可以将类、字段和方法使用短无意义的名称进行重命名。

### 在build.gradle中的配置

    android {
      buildTypes {
      release {
         runProguard true
         proguardFiles getDefaultProguardFile('proguard-android.txt')，'some-other-rules.txt'
       }
     }
    }

##### 哪些东西不需要混淆
 - 反射用到的类
 - Android中Manifest中配置的类(Activity、Service等的子类及Framework类默认不进行混淆)
 - Jni中调用的类
 - 用到的第三方的jar包
 - 表示保留本地的bean文件下的实体类
 - 特别处理js与本地原生组件之间的调用过程
 - 自定义不混淆的类

##### ProGuard语法
 - include {filename}    从给定的文件中读取配置参数
 - basedirectory {directoryname}    指定基础目录为以后相对的档案名称
 - injars {class_path}    指定要处理的应用程序jar,war,ear和目录   
 - outjars {class_path}    指定处理完后要输出的jar,war,ear和目录的名称   
 - libraryjars {classpath}    指定要处理的应用程序jar,war,ear和目录所需要的程序库文件   
 - dontskipnonpubliclibraryclasses    指定不去忽略非公共的库类。   
 - dontskipnonpubliclibraryclassmembers    指定不去忽略包可见的库类的成员。

##### 保留选项
 - keep {Modifier} {class_specification}    保护指定的类文件和类的成员   
 - keepclassmembers {modifier} {class_specification}    保护指定类的成员，如果此类受到保护他们会保护的更好  
 - keepclasseswithmembers {class_specification}    保护指定的类和类的成员，但条件是所有指定的类和类成员是要存在。   
 - keepnames {class_specification}    保护指定的类和类的成员的名称（如果他们不会压缩步骤中删除）   
 - keepclassmembernames {class_specification}    保护指定的类的成员的名称（如果他们不会压缩步骤中删除）   
 - keepclasseswithmembernames {class_specification}    保护指定的类和类的成员的名称，如果所有指定的类成员出席（在压缩步骤之后）   
 - printseeds {filename}    列出类和类的成员-keep选项的清单，标准输出到给定的文件  

##### 实例
(一般第三方包的主页会有关于该项目的混淆规范，如果不确定的话。)



    #指定压缩级别
    -optimizationpasses 5
    #使用大小写混合
    -dontusemixedcaseclassnames
    #混淆第三方jar
    -dontskipnonpubliclibraryclasses
    #混淆时预验
    -dontpreverify
    #混淆记录日志
    -verbose
    #混淆时采用的代码
    -optimizations !code/simplification/arithmetic,!field/*,!class/merging/*

    #support-v4
    -keep class android.support.v4.** { *; }
    -keep public class * extends android.support.v4.**

    #四大组件,Fragment
    -keep public class * extends android.app.Activity
    -keep public class * extends android.app.Application
    -keep public class * extends android.app.Service
    -keep public class * extends android.content.BroadcastReceiver
    -keep public class * extends android.content.ContentProvider
    -keep public class * extends android.app.Fragment

    #因为FastJson使用了序列化，所以不能混淆序列化接口
    -keep public class * implements java.io.Serializable {
        public *;
    }

    #Serializable,enum
    -keepclassmembers class * implements java.io.Serializable {
        static final long serialVersionUID;
        private static final java.io.ObjectStreamField[] serialPersistentFields;
        private void writeObject(java.io.ObjectOutputStream);
        private void readObject(java.io.ObjectInputStream);
        java.lang.Object writeReplace();
        java.lang.Object readResolve();
    }

    # Parcelable
    -keep class * implements android.os.Parcelable {
        public static final android.os.Parcelable$Creator *;
    }

    ## FastJson
    #-dontwarn android.support.**
    #-dontwarn com.alibaba.fastjson.**
    #-dontskipnonpubliclibraryclassmembers
    #-dontskipnonpubliclibraryclasses
    #-libraryjars libs/fastjson.jar
    #-keep class com.alibaba.fastjson.** { *; }

    #资源类
    -keepclassmembers class **.R$* {
        public static <fields>;
    }

    #保持注解，及使用注解的Activity不被混淆
    -keep class * extends java.lang.annotation.Annotation {*;}
    -keep class com.otb.designerassist.activity.** {*;}

    ###混淆FastJson
    -dontwarn com.alibaba.fastjson.**
    -keep class com.alibaba.fastjson.** {*;}

    # EventBus
    -keepclassmembers class ** {
        public void onEvent*(**);
    }

    # 自定义组件
    -keep public class * extends android.view.View {
        public <init>(android.content.Context);
        public <init>(android.content.Context, android.util.AttributeSet);
        public <init>(android.content.Context, android.util.AttributeSet, int);
        public void set*(...);
    }

    # native 方法
    -keepclasseswithmembernames class * {
        native <methods>;
    }

    # realm
    -keep class io.realm.annotations.RealmModule
    -keep @io.realm.annotations.RealmModule class *
    -keep class io.realm.internal.Keep
    -keep @io.realm.internal.Keep class * { *; }
    -dontwarn javax.**
    -dontwarn io.realm.**

    # 友盟
    -dontshrink
    -dontoptimize
    -dontwarn com.google.android.maps.**
    -dontwarn android.webkit.WebView
    -dontwarn com.umeng.**
    -dontwarn com.tencent.weibo.sdk.**
    -dontwarn com.facebook.**

    -keep enum com.facebook.**
    -keepattributes Exceptions,InnerClasses,Signature
    -keepattributes *Annotation*
    -keepattributes SourceFile,LineNumberTable

    -keep public interface com.facebook.**
    -keep public interface com.tencent.**
    -keep public interface com.umeng.socialize.**
    -keep public interface com.umeng.socialize.sensor.**
    -keep public interface com.umeng.scrshot.**

    -keep public class com.umeng.socialize.* {*;}
    -keep public class javax.**
    -keep public class android.webkit.**

    -keep class com.facebook.**
    -keep class com.umeng.scrshot.**
    -keep public class com.tencent.** {*;}
    -keep class com.umeng.socialize.sensor.**

    -keep class com.tencent.mm.sdk.modelmsg.WXMediaMessage {*;}

    -keep class com.tencent.mm.sdk.modelmsg.** implements com.tencent.mm.sdk.modelmsg.WXMediaMessage$IMediaObject {*;}

    -keep class im.yixin.sdk.api.YXMessage {*;}
    -keep class im.yixin.sdk.api.** implements im.yixin.sdk.api.YXMessage$YXMessageData{*;}

    -keep public class [your_pkg].R$*{
        public static final int *;
    }

    -keep class com.zhelectronic.gchcz.networkpacket
    -keep class com.zhelectronic.gchcz.data


参考 :

- [Android代码混淆之混淆规则](http://blog.csdn.net/fengyuzhengfan/article/details/43876197)
- [日积月累：Proguard进行源代码混淆和崩溃日志反混淆](http://blog.csdn.net/p106786860/article/details/11974863)
- [Android开发实践：利用ProGuard进行代码混淆](http://ticktick.blog.51cto.com/823160/1413066)
- [sourceforge](http://proguard.sourceforge.net/)
- [developer.android.com](http://developer.android.com/tools/help/proguard.html)
