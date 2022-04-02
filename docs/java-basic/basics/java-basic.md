## 1.基础和语法

### 1.1 什么是面向对象？

对象，即人们要进行研究的事物，对象具有状态和行为，用数据来描述对象的状态，用操作来描述对象的行为，对象实现了数据和操作的结合。面向对象，就是在解决问题时，将问题涉及到的事物分解成各个对象，分解的目的不是为了完成一个步骤，而是为了描述一个事物在整个解决问题的步骤中的状态和行为。

### 1.2 面向对象和面向过程的区别？

面向过程，就是在解决问题时，分析出解决问题所需要的步骤，然后用函数来把这些步骤一步一步的实现，使用的时候一个一个的依次调用就可以了。

面向对象，就是在解决问题时，将问题涉及到的事物分解成各个对象，分解的目的不是为了完成一个步骤，而是为了描述一个事物在整个解决问题的步骤中的状态和行为。

### 1.3 面向对象的基本特征？

面向对象的三大基本特征：封装、继承、多态。

**封装(Encapsulation)** 

所谓封装，也就是把客观事物封装成抽象的类，并且类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏。

简单的说，一个类就是一个封装了数据以及操作这些数据的代码的逻辑实体。在一个对象内部，某些代码或某些数据可以是私有的，不能被外界访问。通过这种方式，对象对内部数据提供了不同级别的保护，以防止程序中无关的部分意外的改变或错误的使用了对象的私有部分。

举个例子：

如我们想要定义一个矩形，先定义一个Rectangle类，并为这个类定义一些数据和操作，实现这个实体状态和行为的封装。

```java
/**
* 矩形
*/
class Rectangle {

     /**
      * 设置矩形的长度和宽度
      */
     public Rectangle(int length, int width) {
         this.length = length;
         this.width = width;
     }
    
     /**
      * 长度
      */
     private int length;
    
     /**
      * 宽度
      */
     private int width;
    
     /**
      * 获得矩形面积
      *
      * @return
      */
     public int area() {
         return this.length * this.width;
     }
}    
```

**继承(Inheritance)**

简单的说，继承就是在一个现有类型的基础上，通过增加新的方法或者重定义已有方法（下面会讲到，这种方式叫重写）的方式，产生一个新的类型。

我们在使用JAVA时编写的每一个类都是在继承，因为在JAVA语言中，`java.lang.Object`类是所有类最根本的基类（或者叫父类、超类），如果我们新定义的一个类没有明确地指定继承自哪个基类，那么JAVA就会默认为它是继承自Object类的。

在继承中存在以下规律：  
1)类和抽象类都只能最多继承一个类，或者最多继承一个抽象类，并且这两种情况是互斥的，也就是说它们要么继承一个类，要么继承一个抽象类。  
2)类、抽象类和接口在继承接口时，不受数量的约束，理论上可以继承无限多个接口。当然，对于类来说，它必须实现它所继承的所有接口中定义的全部方法。    
3)抽象类继承抽象类，或者实现接口时，可以部分、全部或者完全不实现父类抽象类的抽象（abstract）方法，或者父类接口中定义的接口。    
4)类继承抽象类，或者实现接口时，必须全部实现父类抽象类的全部抽象（abstract）方法，或者父类接口中定义的全部接口。    

继承给我们的编程带来的好处就是对原有类的复用（重用）。就像模块的复用一样，类的复用可以提高我们的开发效率，实际上，模块的复用是大量类的复用叠加后的效果。除了继承之外，我们还可以使用组合的方式来复用类。所谓组合就是把原有类定义为新类的一个属性，通过在新类中调用原有类的方法来实现复用。

使用继承和组合复用原有的类，都是一种增量式的开发模式，这种方式带来的好处是不需要修改原有的代码，因此不会给原有代码带来新的BUG，也不用因为对原有代码的修改而重新进行测试，这对我们的开发显然是有益的。因此，如果我们是在维护或者改造一个原有的系统或模块，尤其是对它们的了解不是很透彻的时候，就可以选择增量开发的模式，这不仅可以大大提高我们的开发效率，也可以规避由于对原有代码的修改而带来的风险。

继承是指这样一种能力：它可以使用现有类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展。

通过继承创建的新类称为“子类”或“派生类”，被继承的类称为“基类”、“父类”或“超类”。继承的过程，就是从一般到特殊的过程。

举个栗子：

我们想要定义一个正方形，因为正方形是长方形的一种特例，我们可以通过继承Rectangle类的方式，来直接获得长方形和正方形共有的特性，而不必重写代码实现。

```java
/**
 * 正方形，继承自矩形
 */
class Square extends Rectangle {

    /**
     * 设置正方形边长
     *
     * @param length
     */
    public Square(int length) {
        super(length, length);
    }
}
```

**多态(Polymorphism)**

定义：所谓多态就是指一个类实例的相同方法，在作用于不同的对象时，可以有不同的解释，或者说产生不同的执行结果。

多态应该是一种运行期的状态，多态机制使具有不同内部结构的对象可以共享相同的外部接口。

这意味着，虽然针对不同对象的具体操作不同，但通过一个公共的类，它们（那些操作）可以通过相同的方式予以调用。

最常见的多态就是将子类传入父类参数中，运行时调用父类方法时通过传入的子类决定具体的内部结构或行为。

1）多态的必要条件

为了实现运行期的多态，或者说是动态绑定，需要满足三个条件：

* 有类继承或者接口实现；
* 子类要重写父类的方法；
* 父类的引用指向子类的对象；

2）多态示例

```java
package audition.polym;

public class CarShop {
	// 售车收入
	private int money = 0;

	// 卖出一部车
	public void sellCar(Car car) {
		System.out.println("车型：" + car.getName() + " 单价：" + car.getPrice());
		// 增加卖出车售价的收入
		money += car.getPrice();
	}

	// 售车总收入
	public int getMoney() {
		return money;
	}

	public static void main(String[] args) {
		CarShop aShop = new CarShop();
		// 卖出一辆宝马
		aShop.sellCar(new BMW());
		// 卖出一辆奇瑞QQ
		aShop.sellCar(new CheryQQ());
		System.out.println("总收入：" + aShop.getMoney());
	}
}

class BMW implements Car {

	public String getName() {
		return "BMW";
	}

	public int getPrice() {
		return 300000;
	}

}

class CheryQQ implements Car {
	public String getName() {
		return "CheryQQ";
	}

	public int getPrice() {
		return 20000;
	}
}
```

以上代码的运行结果：

```
车型：BMW 单价：300000
车型：CheryQQ 单价：20000
总收入：320000
```

在main方法中可以看到，`CarShop`的实例，在作用于不同对象时，会产生不同的运行结果。

继承是多态得以实现的基础。从字面上理解，多态就是一种类型（都是Car类型）表现出多种状态（宝马汽车的名称是BMW，售价是300000；奇瑞汽车的名称是CheryQQ，售价是2000）。
将一个方法调用同这个方法所属的主体（也就是对象或类）关联起来叫做绑定，分前期绑定和后期绑定两种。下面解释一下它们的定义：

* 前期绑定：在程序运行之前进行绑定，由编译器和连接程序实现，又叫做静态绑定。比如static方法和final方法，注意，这里也包括private方法，因为它是隐式final的。
* 后期绑定：在运行时根据对象的类型进行绑定，由方法调用机制实现，因此又叫做动态绑定，或者运行时绑定。除了前期绑定外的所有方法都属于后期绑定。

多态就是在后期绑定这种机制上实现的。多态给我们带来的好处是消除了类之间的耦合关系，使程序更容易扩展。比如在上例中，新增加一种类型汽车的销售，只需要让新定义的类继承Car类并实现它的所有方法，而无需对原有代码做任何修改，`CarShop`类的`sellCar(Car car)`方法就可以处理新的车型了。新增代码如下：

```java
// 桑塔纳汽车
class Santana implements Car {

    public String getName() {
    return "Santana";
    }
    public int getPrice() {
    return 80000;
    }
}
```
### 1.4 重写(Overriding）和重载(Overloading)的区别？

区别重写和重载，首先要弄清楚一个概念，就是什么是方法的型构？
型构就是方法组成结构：具体包括方法的名称和参数，涵盖参数的数量、类型以及出现的顺序，但是不包括方法的返回值类型，访问权限修饰符，以及abstract、static、final等修饰符。

再比较他们的区别：

• 重写，英文名是overriding，是指在继承情况下，子类中定义了与其基类中方法具有相同型构的新方法，就叫做子类把基类的方法重写了。这是实现多态必须的步骤。    
• 重载，英文名是overloading，是指在同一个类中定义了一个以上具有相同名称，但是型构不同的方法。在同一个类中，是不允许定义多于一个的具有相同型构的方法的。

方法的重载和重写都是实现多态的方式，区别在于前者实现的是编译时的多态性，而后者实现的是运行时的多态性。重写要求子类被重写方法与父类被重写方法有相同的返回类型，重载对返回类型没有特殊的要求。

**补充：** 

重写和重载的语法：

重写是子类型定义与父类型“一样的方法”，目的子类修改父类的方法！最重要的目的是“修改”！修改以后，在运行期间执行子类对象的方法，父类型的方法被替换掉了(修改了)。语法:      
1)子类方法名与父类一样, 参数相同     
2)修饰词可以放大, 异常可以具体化(小类型)     
3)父类的私有方法, 不能继承, 就不能被重写!     
4)运行期间动态调用对象类型的方法     

重载是类中(含父子类型) 方法名一样，功能类似，参数不同的方法，语法:   
1)方法名一样, 参数列表不同的方法；  
2)目的是使API的设计更加优雅；   
3)根据方法名和参数的类型调用对应的方法；    

### 1.5 Java的继承与实现

前面我们提到过面向对象有三个特征：封装、继承、多态。

**继承**可以使用现有类的所有功能，并在无需重新编写相同代码的情况下对这些功能进行扩展。这种派生方式体现了`传递性`，在Java中，除了继承，**实现**也体现了功能的`传递性`。

继承和实现两者的明确定义和区别如下：

>继承（Inheritance）：如果多个类的某个部分的功能相同，那么可以抽象出一个类出来，把他们的相同部分都放到父类里，让他们都继承这个类。
>
>实现（Implement）：如果多个类处理的目标是一样的，但是处理的方法方式不同，那么就定义一个接口，也就是一个标准，让他们的实现这个接口，各自实现自己具体的处理方法来处理那个目标。

继承指的是一个类（称为子类、子接口）继承另外的一个类（称为父类、父接口）的功能，并可以增加它自己的新功能的能力。所以，继承的根本目的是要复用现有功能，而实现的根本原因是需要定义一个标准。

在Java中，继承使用`extends`关键字实现，而实现通过`implements`关键字。

特别需要注意的是，Java中支持一个类同时实现多个接口，但是不支持同时继承多个类。但是这个问题在Java 8之后也不绝对了。

另外，在接口中只能定义全局常量（static final）和无实现的方法（Java 8以后可以有defult方法）；而在继承中可以定义属性方法，变量，常量等。

### 1.6 Java的继承与组合

### 1.7 构造函数与默认构造函数

### 1.8 类变量、成员变量和局部变量

### 1.9 成员变量和方法作用域

### 1.10 Java中的显式类型转换和隐式类型转换？

https://blog.csdn.net/s10461/article/details/53941091

https://juejin.cn/post/6844903917835419661

### 1.11 Java 中 java.lang.Void 和 void 有什么作用和区别？




## 2.集合

### 2.1 Collection和Collections区别？

Collection是`java.util`下的接口，它是各种集合的父接口，继承于它的接口主要有`Set`和`List`；

Collections是个`java.util`下的类，是针对集合的帮助类，提供一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。


### 2.2 Set和List区别？

List和Set都是继承自Collection接口。都是用来存储一组相同类型的元素的。

**List特点：** 元素有放入顺序，元素可重复。有顺序，即先放入的元素排在前面。

**Set特点：** 元素无放入顺序，元素不可重复。无顺序，即先放入的元素不一定排在前面。 不可重复，即相同元素在set中只会保留一份。所以，有些场景下，set可以用来去重。 不过需要注意的是，set在元素插入时是要有一定的方法来判断元素是否重复的。这个方法很重要，决定了set中可以保存哪些元素。

### 2.3 ArrayList和LinkedList的区别？

①.Arraylist是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构。  
②.对于随机访问get和set，ArrayList优于LinkedList，因为LinkedList要移动指针。  
③.对于新增和删除操作add和remove，LinkedList比较占优势，因为ArrayList要移动数据。  
④.查找操作IndexOf,lastIndexOf,contains等，两者差不多。  
⑤.随机查找指定节点的操作get,ArrayList速度要优于LinkedList。  

### 2.4 HashSet和TreeSet的区别？

在Java的Set体系中，根据实现方式不同主要分为两大类：HashSet和TreeSet。

1)TreeSet 是二叉树实现的，TreeSet中的数据是自动排好序的，不允许放入null值。  
2)HashSet 是哈希表实现的，HashSet中的数据是无序的，可以放入null值，但只能放入一个null，两者中的值都不能重复，就如数据库中的唯一约束。

### 2.5 Set如何保证元素不重复？

在HashSet中，基本的操作都是由HashMap底层实现的，因为HashSet底层是用HashMap存储数据的。当向HashSet中添加元素的时候，首先计算元素的hashCode值，然后通过扰动计算和按位与的方式计算出这个元素的存储位置，如果这个位置为空，就将元素添加进去；如果不为空，则用equals方法比较元素是否相等，相等就不添加，否则找一个空位添加。

TreeSet的底层是TreeMap的keySet()，而TreeMap是基于红黑树实现的，红黑树是一种平衡二叉查找树，它能保证任何一个节点的左右子树的高度差不会超过较矮的那棵的一倍。

TreeMap是按key排序的，元素在插入TreeSet时compareTo()方法要被调用，所以TreeSet中的元素要实现Comparable接口。TreeSet作为一种Set，它不允许出现重复元素。TreeSet是用compareTo()来判断重复元素的。



## 3.线程

### 3.1多线程的实现方式有几种？不同实现方式之间有什么区别？

主要有两种：

* 继承Thread类的方式；
* 实现Runable接口的方式；

主要区别是：实现Runable接口避免Java的单继承特性带来的局限，比如你的类本来已经`extends`另一个类了。

下面是具体的实现类代码和启动方式。

#### 3.1.1 继承Thread类的方式

```java
public class ThreadDemo01 extends Thread {

    public ThreadDemo01() {
        //编写子类的构造方法，可缺省
    }

    public void run() {
        // 线程具体执行的代码，需根据自己的情况实现
        System.out.println(Thread.currentThread().getName());
    }

    public static void main(String[] args) {
        ThreadDemo01 threadDemo01 = new ThreadDemo01();
        threadDemo01.setName("我是自定义的线程01");

        // 启动线程
        threadDemo01.start();
        System.out.println(Thread.currentThread());
    }
}
```

上述代码的结果打印信息：

```
Thread[main,5,main]-- thread main
我是自定义的线程01
```

#### 3.1.2 实现Runable接口的方式

```java
public class ThreadDemo02 implements Runnable {

    @Override
    public void run() {

        System.out.println(Thread.currentThread().getName());
        int sum = 0;
        for (int i = 1; i <= 100; i++) {
            if (i % 2 == 0){
                sum += i;
            }
        }

        System.out.println(sum);
    }

    public static void main(String[] args) {
        ThreadDemo02 task = new ThreadDemo02();

        Thread thread02 = new Thread(task);
        thread02.setName("我是自定义的线程02");

        // 启动线程
        thread02.start();
        System.out.println(Thread.currentThread() + "--thread main");
    }
}
```

上述代码的结果打印信息：

```
Thread[main,5,main]--thread main
我是自定义的线程02
2550
```

#### 3.1.3 内部类实现线程

这不算线程实现的新方式，只是使用内部类的方式来实现，算是继承Thread类和实现Runnable接口这两种方式的变体。这种形式适用于比如我们的线程就想执行一次，以后就用不到了。

```java
public class ThreadDemo03 {

    public static void main(String[] args) {
        new ThreadDemo03().runTask();
    }

    public void runTask() {
        InnerTreadClass01 thread03 = new InnerTreadClass01();
        thread03.setName("我是自定义的线程03");
        thread03.start();

        InnerTreadClass02 class02 = new InnerTreadClass02();
        Thread thread04 = new Thread(class02);
        thread04.setName("我是自定义的线程04");
        thread04.start();

    }

    class InnerTreadClass01 extends Thread {
        public void run() {
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            // 线程具体执行的代码，需根据自己的情况实现
            System.out.println(Thread.currentThread().getName());
        }
    }

    class InnerTreadClass02 implements Runnable {

        @Override
        public void run() {
            // 线程具体执行的代码，需根据自己的情况实现
            System.out.println(Thread.currentThread().getName());
        }
    }

}
```

上述代码的结果打印信息：

```
我是自定义的线程04
我是自定义的线程03
```


#### 3.1.4 获取线程线程返回结果(通过Callable和FutureTask创建线程)

`FutureTask`本质上是`Runnable`的实现类，它可以接受一个回调作为参数。

```java
public class ThreadDemo04 {

    public static void main(String[] args) {

        Callable<Integer> oneCallable = new Callable<Integer>() {
            @Override
            public Integer call() {
                System.out.println(Thread.currentThread().getName());
                Random random = new Random();

                Integer result = random.nextInt(100);
                return result;
            }
        };

        // 由Callable<Integer>创建一个FutureTask<Integer>对象：
        FutureTask<Integer> oneTask = new FutureTask<>(oneCallable);

        // 启动线程
        Thread oneThread = new Thread(oneTask);
        oneThread.setName("我是自定义的callable线程");
        oneThread.start();

        try {
            Integer m = oneTask.get();
            // 获取线程执行结果
            System.out.println(m);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }


        System.out.println(Thread.currentThread() + "--thread main");

    }
}
```

上述代码的结果打印信息：

```
我是自定义的callable线程
12
Thread[main,5,main]--thread main
```


#### 3.1.5 通过线程池创建线程

频繁的创建和销毁线程是非常消耗资源的。因此可以通过线程池的方式，指定线程池的大小，重复利用空闲线程，减少创建和销毁线程的次数。

```java
public class ThreadDemo05 {

    /* 线程池数量 */
    private static int POOL_NUM = 5;

    public static void main(String[] args) {
        new ThreadDemo05().runTask();
    }

    public void runTask() {
        ExecutorService executorService = Executors.newFixedThreadPool(POOL_NUM);

        for (int i = 0; i < 30; i++) {
            RunnableThread thread = new RunnableThread();

            //Thread.sleep(2000);
            executorService.execute(thread);
        }
        //关闭线程池
        executorService.shutdown();
    }

    class RunnableThread implements Runnable {
        @Override
        public void run() {
            System.out.println("通过线程池方式创建的线程：" + Thread.currentThread().getName() + " ");
        }
    }
}
```

上述代码的结果打印信息：

```
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-3 
通过线程池方式创建的线程：pool-1-thread-4 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-2 
通过线程池方式创建的线程：pool-1-thread-1 
通过线程池方式创建的线程：pool-1-thread-5 
通过线程池方式创建的线程：pool-1-thread-4 
通过线程池方式创建的线程：pool-1-thread-3 
```

从执行结果看，线程是重复利用的。

参考：

1. [java多线程的6种实现方式详解](https://blog.csdn.net/king_kgh/article/details/78213576)；
2. [Java多线程实现的四种方式](https://segmentfault.com/a/1190000023004321)；

### 3.2启动一个线程是用run()还是start()方法?

启动一个线程是调用start()方法，使线程所代表的虚拟处理机处于可运行状态，这意味着它可以由JVM 调度并执行，这并不意味着线程就会立即运行。run()方法是线程启动后要进行回调（callback）的方法。