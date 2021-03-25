# Java基础

## 异常

### 1.异常的概念

异常指在方法不能按照正常方式完成时，可以通过抛出异常的方式退出该方法，在异常中封装了方法执行过程中的错误信息及原因，调用方在获取该异常后可根据业务的情况选择处理该异常或者继续抛出该异常。 在方法在执行过程中出现异常时，Java异常处理机制会将代码的执行权交给异常处理器，异常处理器根据在系统中定义的异常处理规则执行不同的异常处理逻辑（抛出异常或捕捉并处理异常）。

### 2.异常分类

在Java中，Throwable是所有错误或异常的父类，Throwable又可分为Error 和Exception ， 常见的Error 有AWTError 、ThreadDeath ，Exception又可分为RuntimeException和CheckedException。

Error指Java程序运行错误，如果程序在启动时出现Error，则启动失败；如果程序在运行过程中出现Error，则系统将退出进程。出现Error通常是因为系统的内部错误或资源耗尽，Error不能被在运行过程中被动态处理。如果程序出现Error，则系统能做的工作也只能有记录错误的成因和安全终止。 Exception指Java程序运行异常，即运行中的程序发生了人们不期望发生的事件，可以被Java异常处理机制处理。Exception也是程序开发中异常处理的核心，可分为RuntimeException（运行时异常）和CheckedException（检查异常）。

- RuntimeException：指在Java虚拟机正常运行期间抛出的异常， RuntimeException 可以被捕获并处理， 如果出现RuntimeException，那么一定是程序发生错误导致的。我们通常需要抛出该异常或者捕获并处理该异常。常见的RuntimeException 有NullPointerException 、ClassCastException 、ArrayIndexOutOfBundsException等。
- CheckedException ： 指在编译阶段Java 编译器会检查CheckedException异常并强制程序捕获和处理此类异常，即要求程序在可能出现异常的地方通过try catch语句块捕获并处理异常。常见的CheckedException 有由于I/O 错误导致的IOException 、SQLException、ClassNotFoundException等。该类异常一般由于打开错误的文件、SQL语法错误、类不存在等引起。

### 3.异常处理方式：抛出异常、使用try catch捕获并处理异常

异常处理方式有抛出异常和使用try catch语句块捕获并处理异常这两种方式。 （1）抛出异常：遇到异常时不进行具体处理，而是将异常抛给调用者，由调用者根据情况处理。有可能是直接捕获并处理，也有可能是继续向上层抛出异常。抛出异常有三种形式：throws、throw、系统自动抛出异常。其中，throws作用在方法上，用于定义方法可能抛出的异常；throw作用在方法内，表示明确抛出一个异常。

（2）使用try catch捕获并处理异常：使用try catch捕获异常能够有针对性地处理每种可能出现的异常，并在捕获到异常后根据不同的情况做不同的处理。其使用过程比较简单：用try catch语句块将可能出现异常的代码包起来即可。

### 4.throw和throws的区别

- 位置不同：throws作用在方法上，后面跟着的是异常的类；而throw作用在方法内，后面跟着的是异常的对象。
- 功能不同：throws用来声明方法在运行过程中可能出现的异常，以便调用者根据不同的异常类型预先定义不同的处理方式；throw用来抛出封装了异常信息的对象，程序在执行到throw时后续的代码将不再执行，而是跳转到调用者，并将异常信息抛给调用者。也就是说，throw后面的语句块将无法被执行（finally语句块除外）。

## 反射

### 1.反射机制的概念

反射机制指在程序运行过程中，对任意一个类都能获取其所有属性和方法，并且对任意一个对象都能调用其任意一个方法。这种动态获取类和对象的信息，以及动态调用对象的方法的功能被称为Java语言的反射机制。

### 2.反射的应用

Java中的对象有两种类型：编译时类型和运行时类型。编译时类型指在声明对象时所采用的类型，运行时类型指为对象赋值时所采用的类型。 在如下代码中，persion对象的编译时类型为Person，运行时类型为Student，因此无法在编译时获取在Student类中定义的方法：

```html
Person person = new Student();
```

因此，程序在编译期间无法预知该对象和类的真实信息，只能通过运行时信息来发现该对象和类的真实信息，而其真实信息（对象的属性和方法）通常通过反射机制来获取，这便是Java语言中反射机制的核心功能。

### 3.Java的反射API

Java的反射API主要用于在运行过程中动态生成类、接口或对象等信息，其常用API如下。

- Class类：用于获取类的属性、方法等信息。
- Field类：表示类的成员变量，用于获取和设置类中的属性值。
- Method类：表示类的方法，用于获取方法的描述信息或者执行某个方法。
- Constructor类：表示类的构造方法。

### 4.反射的步骤

反射的步骤如下。 （1）获取想要操作的类的Class对象，该Class对象是反射的核心，通过它可以调用类的任意方法。 （2）调用Class对象所对应的类中定义的方法，这是反射的使用阶段。 （3）使用反射API来获取并调用类的属性和方法等信息。 获取Class对象的3种方法如下。 （1）调用某个对象的getClass方法以获取该类对应的Class对象：

```html
Person p =new Person();
Class clazz = p.getClass();
```

（2）调用某个类的class属性以获取该类对应的Class对象：

```html
Class clazz = Person.Class;
```

（3）调用Class类中的forName静态方法以获取该类对应的Class对象，这是最安全、性能也最好的方法：

```html
Class clazz = Class.forName("fullPath");
```

我们在获得想要操作的类的Class对象后，可以通过Class类中的方法获取并查看该类中的方法和属性。

### 5.反射创建对象的两种方式

Class对象的 newInstance()

使用Class对象的newInstance方法创建该Class对象对应类的实例，这种方法要求该Class对象对应的类有默认的空构造器。

调用 Constructor 对象的 newInstance()

先使用Class 对象获取指定的Constructor 对象， 再调用Constructor对象的newInstance方法创建Class对象对应类的实例，通过这种方法可以选定构造方法创建实例。

### 6.Method的invoke方法

Method提供了关于类或接口上某个方法及如何访问该方法的信息，那么在运行的代码中如何动态调用该方法呢？答案就通过调用Method的invoke方法。我们通过invoke方法可以实现动态调用，比如可以动态传入参数及将方法参数化。具体过程为： 获取对象的Method，并调用Method的invoke方法，如下所述。 （1）获取Method对象：通过调用Class对象的getMethod(String name, Class<?>... parameterTypes)返回一个Method对象，它描述了此Class对象所表示的类或接口指定的公共成员方法。name参数是String类型，用于指定所需方法的名称。parameterTypes参数是按声明顺序标识该方法的形参类型的Class 对象的一个数组， 如果parameterTypes为null，则按空数组处理。 （2）调用invoke方法：指通过调用Method对象的invoke方法来动态执行函数。

## 注解

### 1.注解的概念

注解（Annotation）是Java提供的设置程序中元素的关联信息和元数据（MetaData）的方法，它是一个接口，程序可以通过反射获取指定程序中元素的注解对象，然后通过该注解对象获取注解中的元数据信息。

### 2.标准元注解：@Target、@Retention、@Documented、@Inherited

元注解（Meta-Annotation）负责注解其他注解。在Java中定义了4 个标准的元注解类型@Target 、@Retention 、@Documented 、@Inherited，用于定义不同类型的注解。 （1）@Target：@Target说明了注解所修饰的对象范围。注解可被用于packages、types（类、接口、枚举、注解类型）、类型成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（循环变量、catch参数等）。在注解类型的声明中使用了target，可更加明确其修饰的目标。

- TYPE：用于描述类、接口（包括注解类型）或enum声明
- FILED：用于描述域
- METHOD：用于描述方法
- PARAMETER：用于描述参数
- CONSTRUCTOR：用于描述构造器
- LOCAL_VARIABLE：用于描述局部变量
- ANNOTATION_TYPE：用于声明一个注解
- PACKAGE：用于描述包
- TYPE_PARAMATER：对普通变量的声明
- TYPE_USE：能标注任何类型的名称

（2）@Retention：@Retention定义了该注解被保留的级别，即被描述的注解在什么级别有效，有以下3种类型。

- SOURCE：在源文件中有效，即在源文件中被保留。
- CLASS：在Class文件中有效，即在Class文件中被保留。
- RUNTIME：在运行时有效，即在运行时被保留。

（3）@Documented：@Documented表明这个注解应该被javadoc工具记录，因此可以被javadoc类的工具文档化。 （4）@Inherited：@Inherited是一个标记注解，表明某个被标注的类型是被继承的。如果有一个使用了@Inherited修饰的Annotation被用于一个Class，则这个注解将被用于该Class的子类。

### 3.注解处理器

如果没有用来读取注解的方法和工作，那么注解也就不会比注释更有用处了。使用注解的过程中，很重要的一部分就是创建于使用注解处理器。Java SE5 扩展了反射机制的API，以帮助程序员快速的构造自定义注解处理器。

```html
/1：*** 定义注解*/
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface FruitProvider {
    public int id() default -1;
    public String name() default ""；
    public String address() default "";
}
//2：注解使用
public class Apple {
@FruitProvider(id = 1, name = "陕西红富士集团", address = "陕西省西安市延安路")
private String appleProvider;

/3：*********** 注解处理器 ***************/
public static void getFruitInfo(Class<?> clazz) {
    String strFruitProvicer = "供应商信息：";
    Field[] fields = clazz.getDeclaredFields();//通过反射获取处理注解
    for (Field field : fields) {
        if (field.isAnnotationPresent(FruitProvider.class)) {
            FruitProvider fruitProvider = (FruitProvider) field.getAnnotation(FruitProvider.class);
            System.out.println(fruitProvider.id());
        }
    }
}
```

## 内部类

定义在类内部的类被称为内部类。内部类根据不同的定义方式，可分为静态内部类、成员内部类、局部内部类和匿名内部类这4种。

### 1.静态内部类

定义在类内部的静态类被称为静态内部类。

```html
public class Out {
    private static int a;
    private int b;
    public static class Inner {
        public void print() {
            System.out.println(a);
        }
    }
}
```

1. 静态内部类可以访问外部类所有的静态变量和方法，即使是private 的也一样。
2. 静态内部类和一般类一致，可以定义静态变量、方法，构造方法等。
3. 其它类使用静态内部类需要使用“外部类.静态内部类”方式，如下所示：Out.Inner inner =new Out.Inner();inner.print();
4. Java集合类HashMap内部就有一个静态内部类Entry。Entry是HashMap存放元素的抽象，HashMap 内部维护Entry 数组用了存放元素，但是Entry 对使用者是透明的。像这种和外部类关系密切的，且不依赖外部类实例的，都可以使用静态内部类。

### 2.成员内部类

定义在类内部的非静态类，就是成员内部类。成员内部类不能定义静态方法和变量（final 修饰的除外）。这是因为成员内部类是非静态的，类初始化的时候先初始化静态成员，如果允许成员内部类定义静态变量，那么成员内部类的静态变量初始化顺序是有歧义的。

```html
public class Out {
    private static int a;
    private int b;
    public class Inner {
        public void print() {
            System.out.println(a);
            System.out.println(b);
        }
    }
}
```

### 3.局部内部类

定义在方法中的类叫作局部内部类。当一个类只需要在某个方法中使用某个特定的类时，可以通过局部类来优雅地实现。

```html
public class Out {
    private static int a;
    private int b;
    public void test(final int c) {
        final int d = 1;
        class Inner {
            public void print() {
                System.out.println(c);
            }
        }
    }
}
```

### 4.匿名内部类

匿名内部类指通过继承一个父类或者实现一个接口的方式直接定义并使用的类。匿名内部类没有class关键字，这是因为匿名内部类直接使用new生成一个对象的引用。

```html
Test test= new Test();
test.test(new Worker(){
	public int workTime(){
		return 8;
	}});
```

## 泛型

### 1.泛型

泛型的本质是参数化类型，泛型提供了编译时类型的安全检测机制，该机制允许程序在编译时检测非法的类型，比如要实现一个能够对字符串（ String ） 、整形（ Int ） 、浮点型（ Float ） 、对象（Object）进行大小比较的方法，就可以使用Java泛型。 在不使用泛型的情况下，我们可以通过引用Object类型来实现参数的任意化，因为在Java中Object类是所有类的父类，但在具体使用时需要进行强制类型转换。强制类型转换要求开发者必须明确知道实际参数的引用类型，不然可能引起前置类型转换错误，在编译期无法识别这种错误，只能在运行期检测这种错误（即只有在程序运行出错时才能发现该错误）。而使用泛型的好处是在编译期就能够检查类型是否安全，同时所有强制性类型转换都是自动和隐式进行的，提高了代码的安全性和重用性。

### 2.泛型标记和泛型限定：E、T、K、V、N、?

- E-Element：在集合中使用，表示在集合中存放的元素
- T-Type：表示Java类，包括基本的类和我们自定义的类
- K-Key：表示键，比如Map中的Key
- V-Value：表示值
- N-Number：表示数值类型
- ?：表示不确定的Java类型

类型通配符使用“?”表示所有具体的参数类型，例如List<?>在逻辑上是List<String>、List<Integer>等所有List<具体类型实参>的父类。

### 3.泛型限定

3.1 对泛型上限的限定：<? extends T>

在Java中使用通配符“？”和“extends”关键字指定泛型的上限，具体用法为<? extends T>，它表示该通配符所代表的类型是T类的子类或者接口T的子接口。

3.2 对泛型下限的限定：<? super T>

在Java中使用通配符“？”和“super”关键字指定泛型的下限，具体用法为<? super T>，它表示该通配符所代表的类型是T类型的父类或者父接口。

### 4.泛型方法

泛型方法指将方法的参数类型定义为泛型，以便在调用时接收不同类型的参数。在方法的内部根据传递给泛型方法的不同参数类型执行不同的处理方法。

```html
public static<T> void generalMethod(T... inputArray){
}
```

### 5.泛型类

泛型类指在定义类时在类上定义了泛型，以便类在使用时可以根据传入的不同参数类型实例化不同的对象。 泛型类的具体使用方法是在类的名称后面添加一个或多个类型参数的声明部分，在多个泛型参数之间用逗号隔开。

```html
public class GeneralClass<T>{

}
```

### 6.泛型接口

泛型接口的声明和泛型类的声明类似，通过在接口名后面添加类型参数的声明部分来实现。泛型接口的具体类型一般在实现类中进行声明，不同类型的实现类处理不同的业务逻辑。

```html
public interface IGeneral<T>{
	public T getId();
} 
```

### 7.类型擦除

在编码阶段采用泛型时加上的类型参数，会被编译器在编译时去掉，这个过程就被称为类型擦除。因此，泛型主要用于编译阶段。在编译后生成的Java字节代码文件中不包含泛型中的类型信息。例如，编码时定义的List<Integer> 和List<String> 在经过编译后统一为List。JVM所读取的只是List，由泛型附加的类型信息对JVM来说是不可见的。 Java类型的擦除过程为：首先，查找用来替换类型参数的具体类（该具体类一般为Object），如果指定了类型参数的上界，则以该上界作为替换时的具体类；然后，把代码中的类型参数都替换为具体的类。

## 序列化

### 1.序列化

Java对象在JVM运行时被创建、更新和销毁，当JVM退出时，对象也会随之销毁，即这些对象的生命周期不会比JVM的生命周期更长。但在现实应用中，我们常常需要将对象及其状态在多个应用之间传递、共享，或者将对象及其状态持久化，在其他地方重新读取被保存的对象及其状态继续进行处理。这就需要通过将Java对象序列化来实现。 在使用Java序列化技术保存对象及其状态信息时，对象及其状态信息会被保存在一组字节数组中，在需要时再将这些字节数组反序列化为对象。注意，对象序列化保存的是对象的状态，即它的成员变量，因此类中的静态变量不会被序列化。 对象序列化除了用于持久化对象，在RPC（远程过程调用）或者网络传输中也经常被使用。

### 2.Java序列化API的使用

Java序列化API为处理对象序列化提供了一个标准机制，具体的Java系列化需要注意以下事项。

- 类要实现序列化功能，只需实现java.io.Serializable接口即可。
- 序列化和反序列化必须保持序列化的ID 一致，一般使用private static final long serialVersionUID定义序列化ID。
- 序列化并不保存静态变量。
- 在需要序列化父类变量时，父类也需要实现Serializable接口。
- 使用Transient关键字可以阻止该变量被序列化，在被反序列化后，transient变量的值被设为对应类型的初始值，例如，int类型变量的值是 0，对象类型变量的值是null。

注意，transient修饰的属性和static修饰的静态属性不会被序列化。 对象通过序列化后在网络上传输时，基于网络安全，我们可以在序列化前将一些敏感字段（用户名、密码、身份证号码）使用秘钥进行加密，在反序列化后再基于秘钥对数据进行解密。这样即使数据在网络中被劫持，由于缺少秘钥也无法对数据进行解析，这样可以在一定程度上保证序列化对象的数据安全。

### 3.序列化和反序列化

可以基于JDK原生的ObjectOutputStream和ObjectInputStream类实现对象进行序列化及反序列化，并调用其writeObject和readObject方法实现自定义序列化策略。

```html
FileInputStream fis=new FileInputStream("out");
ObjectInputStream ois=new ObjectInputStream(fis);
Worker worker=(Worker)ois.readObject():
```