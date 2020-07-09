## equals & ==

+ == 
  + 基本数据类型（也称原始数据类型）：byte,short,char,int,long,float,double,boolean。他们之间的比较，应用双等号（==）, 比较的是他们的值。
  + 引用数据类型：当他们用（==）进行比较的时候，比较的是他们在内存中的存放地址（确切的说，是堆内存地址。
+ equals 继承于Object

````java
// equals默认实现
public boolean equals(Object obj) {
    //this - s1
    //obj - s2
    return (this == obj);
}
````

````java
class Value {
    int i;
}

public class EqualsMethod {
    public static void main(String[] args) {
        
        // test default equals
        Value v1 = new Value();
        Value v2 = new Value();
        v1.i = v2.i = 100;
        System.out.println(v1.equals(v2));  // false, 因为Value中没有equals的实现
        
        // test string
        String str1 = "Hello";
        String str2 = new String("Hello");
        System.out.println(str1 == str2); // false, 两个String的堆地址不一样
        String str1 = str2;
        System.out.println(str1 == str2); // true
        
        
        // Integer 内部维护着一个 IntegerCache 的缓存，
        // 默认缓存范围是 [-128, 127]，所以 [-128, 127] 之间的值用 == 和 != 比较也能能到正确的结		  // 果，但是不推荐用关系运算符比较。
        // test Integer one
        Integer n1 = 47;
        Integer n2 = 47;
        System.out.println(n1 == n2);  // true
        
        //test Integer two
        Integer n1 = 128;
        Integer n2 = 128;
        System.out.println(n1 == n2);  // false, 两个Integer的堆地址不一样
    }
}

````

## 移位运算符

+ << n

左移n位，右边补0

+ \>> n

右移n位，若值为正，则在高位插入 0；若值为负，则在高位插入 1。

+ \>\>> n

右移n位，左边补0

| 正数（第一位为符号位，为0） | 负数（第一位为符号位，为1）                                  |
| --------------------------- | ------------------------------------------------------------ |
| 原码 = 反码 = 补码          | 反码 = 原码符号位不变，其余取反<br />补码 = 反码符号不变，其余位+1 |

## 权限控制

+ **public**

access by all (class, package, subclass, world)

+ **protected**

access by class 、package & subclass

+ **default**

access by class & package

+ **private**

access by class

