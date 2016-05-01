# 第三章 对于所有对象都通用的方法

Object类主要为了**拓展**，它的所有非final方法都被设计成覆盖。在覆盖这些方法时，都有责任遵守一些通用的约定，如果不能做到，其他依赖于这些约定的类(比如HashMap和HashSet)将无法正常的工作。


### 第8条：覆盖equals时请遵守通用约定
不覆盖equals方法的情况下，类的每个实例只与它自身相当，如果满足了一下任何一个条件，那正是所期望的 ：

 - 类的每个实例本质上都是唯一的
 - 不关心类是否提供了“逻辑相等”的测试功能
 - 超类已经覆盖了equals方法，从超类继承过来的行为对子类也是有效的
 - 类时私有或者包级私有的，可以确定它的equals方法永远不会被调用
 
 
 ##### 什么时候应该覆盖Object.equals方法
 如果类具有自己特有的“逻辑相等”概念，而且超类还没有覆盖equals以实现期望的行为。
  
  > 逻辑相等，可以理解为“值”相等
   
##### 高质量equal的实现

 1. 使用==操作符检查 “参数是否为这个对象的引用”
 - 使用instanceof操作符检查 “参数是否是正确的类型”
 - 把参数转换成正确的类型
 - 对于该类中单每个“关键”域，检查参数的域是否与该对象中对应的域相匹配
 - 编写完equals方法之后，问三个问题：它是否是对称的、传递的、一致的？
 - 不要讲equals中的Object对象替换成其他的类型
 
    public boolean equals(MyClass o) { ... }
  
eg :

    public class Student {

        private String name;
        private int age;

        @Override
        public boolean equals(Object obj) {

            //1.检查对象是否为null，如果对象为null，则始终返回false
            if (obj == null){
                return false;
            }

            //2.检查参数是否是正确的类型
            if (obj instanceof Student){
                //如果是正确的类型，则进行强制类型转换
                Student student = (Student) obj;
                //紧着，参数的判断
                if (name.equals(student.name) && name.equals(student.age)){
                    return true;
                }
            }

            return false;
        }
    }
    
    
### 第9条： 覆盖equals总要覆盖hashCode
在每个覆盖了equals方法中的类中，也必须覆盖hashCode方法，如果不这样的话，就会违反Object.hashCode的通用规定，从而导致该类无法结合所有基于散列的集合一起正常工作。

##### Object中规范 

 - 只要equals方法操作所用到的信息没有修改，那么在对同一个对象调用多次，hashCode方法必须返回同一个整数
 - 如果两个对象通过equals方法比较相等，则hashCode必须产生同样的整数结果。反之，如果equals比较不相等，则hashCode不一样要返回不一样的结果
 
 
### 第10条： 始终覆盖toString
Object中默认的toString方法 ：

    public String toString() {
         return getClass().getName() + '@' + Integer.toHexString(hashCode());
    }

它只有类名+'@'+散列值，在实际应用中，toString方法应该返回**对象中包含的所有值得关注的信息**。

###第11、12条： 谨慎的使用clone、考虑实现Comparable接口
(没怎么用过，不怎么看的明白。)




















    