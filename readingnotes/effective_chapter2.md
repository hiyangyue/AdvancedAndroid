#  第二章 创建和销毁对象

**章节主题** 
- 何时以及如何创建对象；
- 何时以及如何避免创建对象；
- 如何确保对象适时的销毁
- 如何管理销毁对象之前必要的清理动作
 
### 1.考虑使用静态工厂方法替代构造器
静态工厂方法只是**一个返回类的实例的静态方法**，它和普通的静态方法一样。

Java源码中的一个实例 ：

    public static Boolean valueOf(boolean b){
        return b ? Boolean.TRUE : Boolean.FALSE;
    }

相比于构造器，静态工厂方法的优势 ：

 - **有名称**
      
   构造方法本身没有名称，不能确切的返回描述返回的对象，具有适当名称的静态工厂方法更容易被使用，产生的代码也更容易被阅读。
 - **不必每次调用的时候创建一个新的实例**
 
   构造方法的每次调用都会重新创建一个新的实例，而静态工厂方法可以预先构建好实例/实例缓存，进行重复利用，从而避免不必要的重复对象的创建。
 
 - **能返回原返回类型的任何子类型的对象**
 
   构造方法只能返回类本身，而静态方法可以返回它的子类，利用静态使得方法更加灵活。
   
 -  **创建参数化类型实例时，代码更加简洁**
 
Java7之后的版本，已经实现了类型推到，所以这也不算是真正意义上优势。Java 7之前的版本

    Map<String,List<String>> m = new HashMap<Stirng,List<String>>();
   
Java7之后的版本

     Map<String,List<String>> stringListMap = new HashMap<>();
 
书中推荐的实现方式 ： 

    public static <K,V>  HashMap<K,V> getInstance(){
        return new HashMap<K,V>();
    }
    
    //调用
    Map<String,List<String>> m2 = getInstance();
   

静态工厂方法的缺点 ： 
 
 - 类如果不含有或者受保护的构造器，就不能被子类化
 - 它和其他的静态方法并没有实际上的区别
 

静态工厂的命名规范 ：
  
 - valueOf 返回的实例与参数具有相同的值,一般用于类型转换(如String Boolean Double等等)
of

- getInstance 返回的实例工具参数而定,对于单例,则一般没有参数,并且每次返回的都是同一个实例

- newInstance 每次返回一个新的实例

- getType 返回的对象类型

- newType 每次返回不同的Type   
   
   
### 2.遇到多个构造器参数时优先考虑构建器
当实例化一个类时，特别是有很多可选的参数，如果我们考虑使用写很多不同参数的构造方法，就会使得可读性变得很差。

这个时候我们就需要使用到构建器，也就是Builder模式：**不直接生成想要的对象，而是让客户端利用所有必要的参数调用构造器，得到一个Builder对象，然后在Builder对象想，调用类似setter方法，来设置相关可选的参数**。

Builder模式在Android中的应用很常见 ：

    AlertDialog dialog = new AlertDialog.Builder(this)
                .setCancelable(true)
                //...各种可选参数 想要什么设置什么
                .setMessage("Builder模式")
                .create(); 

##### 应用场景
Builder模式的缺点 ：为了创建对象，必须先创建一个Builder对象，虽然这样的开销对性能的影响不明显，但确实是一个多余的开销 。如果类的构造器或者静态方法中有很多参数，且他们中的大多数的参数时可选的时候，优先考虑Builder模式。


### 第3条： 用私有构造器或枚举类型强化Singleton属性
单类型的枚举类型是实现单利模式的最佳方法。下面是枚举类单利的实现方式 ：

    public enum Elvis {
        INSTANCE;
        
        public void read() {...}
    }


枚举单利的优点 ：
 - 防止被多次实例化
 - 防反射
 - 可以序列化
 - 简洁

关于单例模式的写法问题，StackoverFlow上有更详细的套路 ：

 [What is an efficient way to implement a singleton pattern in Java?](http://stackoverflow.com/questions/70689/what-is-an-efficient-way-to-implement-a-singleton-pattern-in-java)


### 第4条： 通过私有构造器强化不可实例化的能力
不可实例化是指当前的类只包含 静态方法和静态域的类。在Java中，只有当类不包含显示的构造器时，编译器才会生成缺省的构造器，因为只要让类包含私有的构造器，它就不能被实例化。

    public class BitmapUtils{
        private BitmapUtils() {
            throw new AssertionError();
        }
    }

### 第5条： 避免创建不必要的对象
1. 最好能够重用对象而不是每次调用的时候就创建一个功能相同的新对象
2. 如果对象是不可变的，它就始终可以重用。(这里与15条的知识点相关)

#### 创建String的实例
使用

    String s = "hello world";

替代

    
    String s = new String("hello world");
   
####  只在初始化的创建对象的实例，而不是每次调用都创建这些是实例
Boolean.valueOf()方法重用了FALSE和TRUE来避免重复创建相同功能的对象

    public static final Boolean TRUE = new Boolean(true);
    public static final Boolean FALSE = new Boolean(false);
  
(这里的实例来自 ：[第5条 避免创建不必要的对象](https://github.com/AlanCheen/OnePiece/blob/master/ReadingNotes%2FEffectiveJava%2F%E7%AC%AC2%E7%AB%A0-%E5%88%9B%E5%BB%BA%E5%92%8C%E9%94%80%E6%AF%81%E5%AF%B9%E8%B1%A1.md#类如果不含公有的或者受保护的构造器就不能被子类化)) 

#### 优先使用基本类型而不是装箱基本类型

    public static void main(String[] args){
        Long sum = 0L;
        
        for(long i=0;i<Integer.MAX_VALUE;i++){
          sum += i;
        }
        
        System.out.println(sum);
    }   
   
sum 的类型为Long,这样会比long多创建约2的31次方的Long实例,影响性能,所需时间大约是long的6倍多。
   
### 第6条： 消除过期对象的引用
内存泄露：**如果一个栈先是增长，然后收缩，那么，从栈中弹出来的对象是不会被当做垃圾回收的，即使使用栈的程序不再引用这些对象，它们也不会被回收**。这是因为，栈内部维护着对这些对象的过期引用。
   
由于过期引用的存在,GC并不会去回收它们,所以需要我们手动清空这些引用。比如 ：

    public Object pop(){
        if(size==0) throw new EmptyStackException();
        Object result = elements[--size];
        elements[size] = null; //Eliminate obsolete reference
        return result;
    }

或者使用弱引用。