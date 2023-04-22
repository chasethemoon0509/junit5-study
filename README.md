<h2 align="center">Junit5 学习笔记</h2>

因为个人理解可能不到位，所以某些内容会出现错误，请参考：

junit5 官方文档：https://junit.org/junit5/docs/current/user-guide/

maven-surefire-plugin 插件相关命令的文档：https://maven.apache.org/surefire/maven-surefire-plugin/

maven-failsafe-plugin 插件相关命令的文档：https://maven.apache.org/surefire/maven-failsafe-plugin/index.html

<p align="center">
<img src="https://img.shields.io/badge/jdk-1.8-brightgreen" />
<img src="https://img.shields.io/badge/junit--jupiter-5.9.2-brightgreen"/>
</p>

<hr>

# 1. 初步了解
## 1.1 项目配置
创建 maven 项目，使用 maven 管理 selenium、junit5 等相关依赖和插件。

maven 添加依赖和插件：
```
<dependencies>
        <!-- ... -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.9.2</version>
            <scope>test</scope>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.seleniumhq.selenium/selenium-java -->
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.8.3</version>
        </dependency>

        <!-- ... -->
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.0.0-M7</version>
            </plugin>
            <plugin>
                <artifactId>maven-failsafe-plugin</artifactId>
                <version>3.0.0-M7</version>
            </plugin>
        </plugins>
    </build>
```

plugins 范围内的表示要引入的插件，maven-surefire-plugin 和 maven-failsafe-plugin 是 maven 用于执行测试的插件，
只有添加了这两个插件才能执行测试用例，

maven-surefire-plugin 用于执行单元测试。

maven-failsafe-plugin 用于执行集成测试。

## 1.2 编写第一个测试用例并执行

在项目的 src 目录下有一个 test/java 目录，测试类要定义在此路径下。

```
public class TestDemo {
    @Test
    void testCase1() {
        System.out.println("执行用例1");
        assertEquals(1, 1);
    }
}
```
执行命令：mvn test


## 1.3 容器

容器是测试树 `test tree` 中的一个节点 `node`，容器中包含其他容器或测试，例如测试类。

通俗地，一个测试类就是一个容器，这个测试类中可以存在内部类，这个内部类也是一个容器，同时这个测试类中也有一些测试方法，这些测试方法就是测试类的子节点。


## 1.4 @Test

此注解将一个普通方法声明为测试方法。通俗地说，被 @Test 注解的方法就是一条测试用例。

## 1.5 测试类

测试类就是上面说到的容器，测试类里面至少要包含一个测试方法。

测试类不能使用 abstract 关键字修饰，即不能是一个抽象类。

测试类中必须要有一个构造方法。

测试类不需要使用 public 关键字修饰，但是也不能使用 private 关键字修饰。

## 1.6 测试方法

测试方法是使用 @Test、@RepeatedTest、@ParameterizedTest、@TestFactory 或 @TestTemplate 直接注释或元注释的任何实例方法。

测试方法不能使用 abstract 关键字修饰，即测试方法不能是抽象的。

大部分测试方法不能有返回值，所以返回值类型需要使用 void 关键字。

使用 @TestFactory 注解的测试方法需要有返回值。

测试方法不需要使用 public 关键字修饰，也不能使用 private 关键字修饰。

## 1.7 生命周期方法

使用@BeforeAll、@AfterAll、@BeforeEach 或 @AfterEach 直接注释或元注释的任何方法。

生命周期方法不需要使用 public 关键字修饰，但是也不能使用 private 关键字修饰。

## 1.8 测试类、测试方法、生命周期方法

定义的时候可以省略 public 关键字，当然写上也没问题。

如果你需要在另一个包中调用某个测试类、测试方法、生命周期方法，那就必须要使用 public 关键字修饰。

一个标准的测试类：
```
class TestDemo {

    @BeforeAll
    static void setupClass() {
        System.out.println("类级别的 setup，在当前类中所有测试方法执行之前执行一次");
    }

    @BeforeEach
    void setup() {
        System.out.println("方法级别的 setup，在当前类中每个测试方法执行之前执行一次");
    }

    @Test
    void testCase1() {
        System.out.println("执行用例1");
    }

    @Test
    void testCase2() {
        System.out.println("执行用例2");
    }

    @AfterEach
    void teardown() {
        System.out.println("方法级别的 teardown，在当前类中每个测试方法执行完毕之后执行一次");
    }
    
    @AfterAll
    static void teardownClass() {
        System.out.println("类级别的 teardown，在当前类中所有测试方法执行完毕之后执行一次");
    }
}
```

# 2. 显示名称
## 2.1 @DisplayName

```@DisplayName(value = "显示名称")```

使用此注解可以给测试类、测试方法自定义显示的名称，主要用于在 ide 中显示。

```
@DisplayName("测试类 TestDemo2")
public class TestDemo {
    @Test
    @DisplayName("测试用例 testCase1")
    void testCase1() {
        System.out.println("测试用例 testCase1");
        assertEquals(1, 1);
    }
}
```

ide（此处用的是 idea ） 左下角显示：

![](https://s1.ax1x.com/2023/04/10/ppq54l8.png#pic_center)


不使用 @DisplayName 注解，直接显示类或方法的名称：

[![ppqIXEd.png](https://s1.ax1x.com/2023/04/10/ppqIXEd.png)](https://imgse.com/i/ppqIXEd)




## 2.2 显示名称生成器

上面的显示名称注解 @DisplayName 支持自定义测试类和测试方法在 ide 中执行时显示的名称。

这里的显示名称生成器还可以根据某个行为规则来显示名称，例如使用空格替换测试方法名称中的下划线，执行测试方法的时候，ide 里面就不会显示名称中的下划线。

需要使用 @DisplayNameGeneration 注解和接口 DisplayNameGenerator 实现，且注解只能用于测试类。

@DisplayNameGeneration 注解接收一个 class 类型的参数，而且限定了必须是实现了 DisplayNameGenerator 接口的类。


导入：
```
import org.junit.jupiter.api.DisplayNameGeneration;
import org.junit.jupiter.api.DisplayNameGenerator;
```

jupiter 中提供了几个默认的生成器，例如 Standard、Simple、ReplaceUnderscores、IndicativeSentences

|         生成器         |                                 行为                                 | 
|:-------------------:|:------------------------------------------------------------------:|
|      Standard       |                    自 junit Jupiter 5 以来的标准显示名称                     |
|       Simple        | 如果测试方法没有参数，那么显示的名称只有方法名，没有括号。例如方法 `test_case()`，显示的就只是 `test_case` |
| ReplaceUnderscores  |                           用空格替换方法名中的下划线                            |
| IndicativeSentences |                    将类名和方法名连接在一起显示，组成一个句子，增加阅读性                     |



## 2.3 Standard 生成器
Standard 生成器，其实是 DisplayNameGenerator 接口的实现类。

Standard 类中实现了 DisplayNameGenerator 接口的 generateDisplayNameForClass 方法，
这个方法的源码如下：
```
public String generateDisplayNameForClass(Class<?> testClass) {
    String name = testClass.getName();   // 通过反射获取测试类的类名
    int lastDot = name.lastIndexOf(46);  
    return name.substring(lastDot + 1);  // 返回类名字符串
}
```
通过源码得知测试类的显示名称其实就是类名本身。

另外一个方法 generateDisplayNameForMethod ，是为测试方法生成显示名称，源码：
```
public String generateDisplayNameForMethod(Class<?> testClass, Method testMethod) {
    return testMethod.getName() + DisplayNameGenerator.parameterTypesAsString(testMethod);
}
```
源码中有两部分，第一部分是通过反射获取测试方法的方法名```testMethod.getName()```，
另外一部分是```DisplayNameGenerator.parameterTypesAsString(testMethod);```获取方法的参数类型(还包括方法的括号)，
最后将这两部分拼接成一个完整的字符串并返回。


那么，Standard 所谓的 "自 junit Jupiter 5 以来的标准显示名称"，其实显示的就是测试类和测试方法本身的名称。
用代码来检验一下：
```
@DisplayNameGeneration(DisplayNameGenerator.Standard.class)
class TestDemo1 {
    @ParameterizedTest  // 定义参数化测试
    @ValueSource(ints = {1, 2})  // 定义参数值
    void testCase1(int num) {   // 这个测试方法有参数，看看执行之后 idea 中显示的名称是否包含参数
        assertEquals(1, num);
    }
}
```
代码运行之后，idea 左下角显示如下：

[![ppqLirV.png](https://s1.ax1x.com/2023/04/10/ppqLirV.png)](https://imgse.com/i/ppqLirV)

可以看到测试类显示的名称是 TestDemo1，测试方法显示的名称是 testCase1(int)


## 2.4 Simple 生成器

它也是 DisplayNameGenerator 接口的实现类。

使用它，会删除没有参数的方法的尾随括号，有参数则不会删除。

使用代码验证：
```
@DisplayNameGeneration(DisplayNameGenerator.Simple.class)
class TestDemo {
    // 没有参数的方法，正常情况应该不显示后面的括号
    @Test
    void testCase1() {
        System.out.println("执行测试用例1");
    }

    // 有参数的方法，正常情况应该显示参数类型
    @ParameterizedTest
    @ValueSource(ints = {2})
    void testCase2(int no) {
        System.out.printf("执行测试用例%d", no);
    }
}
```
代码执行后，idea 左下角显示：

[![ppqL5ZT.png](https://s1.ax1x.com/2023/04/10/ppqL5ZT.png)](https://imgse.com/i/ppqL5ZT)

可以看到，没有参数的测试方法 testCase1() 只显示了方法名，没有显示括号。

而有参数的方法 testCase2(int no) 显示了参数类型。


## 2.5 ReplaceUnderscores 生成器
它也是 DisplayNameGenerator 接口的实现类。

使用它，会用空格替换测试类名或测试方法名中的下划线。

代码验证：
```
@DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class)
class Test_Demo {  // 创建一个名称有下划线的测试类
    @Test
    void test_case() {  // 创建一个名称有下划线的测试方法
        System.out.println("执行测试用例");
    }
}
```

代码执行后，显示如下：

[![ppqOJmV.png](https://s1.ax1x.com/2023/04/10/ppqOJmV.png)](https://imgse.com/i/ppqOJmV)

可以看到，测试类显示的名称是 Test Demo，测试方法显示的名称是 test case。


## 2.6 IndicativeSentences 生成器

对于 IndicativeSentences，可以使用 @IndicativeSentencesGeneration 这个注解来自定义分隔符和底层生成器。

它生成的测试方法名是 `测试类名 + 分隔符 + 测试方法名`

底层生成器可以使用上述四种生成器(可能不光这四种，还有自定义的生成器)：Standard、Simple、ReplaceUnderscores、IndicativeSentences

以 Simple 作为底层生成器为例：
```
@IndicativeSentencesGeneration(separator = "——>", generator = DisplayNameGenerator.Simple.class)
class TestDemo {
    // 没有参数的方法，正常情况应该不显示后面的括号
    @Test
    void test_case1() {
        System.out.println("执行测试用例1");
    }

    @Test
    void test_case2() {
        System.out.println("执行测试用例2");
    }
}
```
代码执行之后，idea 显示如下：

[![ppqXC7T.png](https://s1.ax1x.com/2023/04/10/ppqXC7T.png)](https://imgse.com/i/ppqXC7T)



## 2.7 自定义显示名称生成器 —— 直接实现 DisplayNameGenerator 接口

除了 junit5 默认提供的四种显示名称生成器，我们还可以自已定义。

通过上面的笔记，我们可以得知，默认的四种生成器都是实现了 DisplayNameGenerator 接口的类。

所以，如果我们要自己定义，也需要实现 DisplayNameGenerator 接口。

我们需要实现接口中的三个方法：generateDisplayNameForClass、generateDisplayNameForNestedClass、generateDisplayNameForMethod，
这三个方法分别表示给测试类生成显示名称、给嵌套测试类生成显示名称、给测试方法生成显示名称。

我们还需要定义一个方法，用来处理显示名称。

例如，自定义一个生成器，生成器的行为是把类名和方法名使用中括号包裹起来：
```
// 因为对 java 不够熟练、了解的不够深，这个实现类可能不标准或不正确
// 但是基本上可以正常使用
class Bracket implements DisplayNameGenerator {
    @Override
    public String generateDisplayNameForClass(Class<?> aClass) {
        String name = aClass.getName();
        return "[" + name + "]";
    }

    @Override
    public String generateDisplayNameForNestedClass(Class<?> aClass) {
        String simpleName = aClass.getSimpleName();
        return "[" + simpleName + "]";
    }

    @Override
    public String generateDisplayNameForMethod(Class<?> aClass, Method method) {
        return "[" + method.getName() + DisplayNameGenerator.parameterTypesAsString(method) + "]";
    }

}
```

在测试类中使用这个 Bracket 生成器：
```
@IndicativeSentencesGeneration(separator = "——>", generator = Bracket.class)
class TestDemo {
    @Test
    void test_case1() {
        System.out.println("执行测试用例1");
    }

    @Test
    void test_case2() {
        System.out.println("执行测试用例2");
    }
}
```

执行之后，idea 结果显示：

[![ppqvCYF.png](https://s1.ax1x.com/2023/04/11/ppqvCYF.png)](https://imgse.com/i/ppqvCYF)

可以看到测试方法的名称和测试类的名称已经被中括号[]包裹显示了。


## 2.8 自定义显示名称生成器 —— 继承 DisplayNameGenerator 接口的实现类

这种方式是对 DisplayNameGenerator 接口的实现类进行拓展，也就是对上面说到的默认的四种生成器进行拓展。

因为 DisplayNameGenerator 接口的实现类是普通的类，所以我们要使用 extends 关键字来继承它们。

继承之后，我们要重写它们的方法。

例如，定义一个生成器，继承 Standard， 生成器的行为是在测试类的名称和测试方法的名称前面加一个 emoji。

```
class Emoji extends DisplayNameGenerator.Standard {
    @Override
    public String generateDisplayNameForClass(Class<?> testClass) {
        return "✨" + super.generateDisplayNameForClass(testClass);
    }

    @Override
    public String generateDisplayNameForNestedClass(Class<?> nestedClass) {
        return "⚔" + super.generateDisplayNameForNestedClass(nestedClass);
    }

    @Override
    public String generateDisplayNameForMethod(Class<?> testClass, Method testMethod) {
        return "🏹" + super.generateDisplayNameForMethod(testClass, testMethod);
    }
}
```

在测试类中使用这个 Emoji 生成器：
```
@DisplayNameGeneration(Emoji.class)
class TestDemo {
    @Test
    void test_case1() {
        System.out.println("执行测试用例1");
    }

    @Test
    void test_case2() {
        System.out.println("执行测试用例2");
    }
}
```

代码执行后，idea 里显示：

[![ppqvj9e.png](https://s1.ax1x.com/2023/04/11/ppqvj9e.png)](https://imgse.com/i/ppqvj9e)

可以看到，测试类和测试方法名称前面都有一个 emoji。


最后：自定义的生成器都可以在 @DisplayNameGeneration 、@IndicativeSentencesGeneration 这两个注解中被使用。



## 2.9 设置默认显示名称生成器

在整个项目级别上，设置默认显示名称生成器，设置以后，会在所有的测试类和测试方法中生效，不需要额外的在测试类中单独声明。

设置方法：
1. 在 src/test/ 目录下创建一个 resources 目录
2. 在 resources 目录中添加一个 junit-platform.properties 文件
3. 在文件中输入内容：
```
junit.jupiter.displayname.generator.default = \
    org.junit.jupiter.api.DisplayNameGenerator$ReplaceUnderscores
```
内容里面，等号右边，$ 符号后面的就是你要设置的生成器，`$ReplaceUnderscores`，我们可以替换成其他的默认生成器和自定义的生成器。

我们就使用 ReplaceUnderscores，还记得吗，这个生成器是把下划线替换成空格，来看看这个设置是否生效：
```
// 创建两个包，每个包内有一个测试类，每个测试类中有两个测试方法，测试类和测试方法名称都有下划线

// 1. study1 包内的测试类代码
class Test_Demo1 {
    @Test
    void test_case1() {
        System.out.println("执行测试用例1");
    }

    @Test
    void test_case2() {
        System.out.println("执行测试用例2");
    }
}


// 2. study2 包内的测试类代码
class Test_Demo2 {
    @Test
    void test_case1() {
        System.out.println("执行测试用例1");
    }

    @Test
    void test_case2() {
        System.out.println("执行测试用例2");
    }
}

```

在 idea 中，打开测试类 Test_Demo1 ，点击执行按钮，执行之后的结果显示：

[![ppqxQEV.png](https://s1.ax1x.com/2023/04/11/ppqxQEV.png)](https://imgse.com/i/ppqxQEV)

可以看到显示的名称中没有下划线，而是空格，而且我们在代码中也没有单独使用 @DisplayName 等显示名称相关的注解，看来这个设置是成功了。

我们在到另一个包中执行测试类 Test_Demo2：

[![ppqx458.png](https://s1.ax1x.com/2023/04/11/ppqx458.png)](https://imgse.com/i/ppqx458)

可以看到，这个测试类和测试方法显示的名称中也没有下划线，也是空格。



## 2.10 把自定义的生成器设置成默认生成器

上面那个部分，我们是把 junit5 提供的生成器设置成全局默认的生成器，下面我们把自己定义的生成器设置成全局默认的生成器。

还是使用上面自定义的 Emoji 生成器，把 junit-platform.properties 文件中的内容修改为：
```
junit.jupiter.displayname.generator.default = study1.Emoji

# 需要注意的是，如果等号右边的是自定义的生成器，那么就需要写这个生成器的完全限定类名，即类名的全称，从包名开始
# 例如 com.xxx.xxx.Demo，其中 com.xxx.xxx 是包名，Demo 是类名
```

然后，其他地方不用修改，直接执行测试类，idea 中显示结果如下：

[![ppqxLbq.png](https://s1.ax1x.com/2023/04/11/ppqxLbq.png)](https://imgse.com/i/ppqxLbq)



## 2.11 总结

> 关于显示名称注解的优先级：我们上面说到了好几种显示名称注解，例如 @DisplayName、@DisplayNameGeneration，
> 除了注解之外，还有设置全局的默认显示名称生成器，这些方式中，大体的优先级如下：
> - 优先级最高的是 @DisplayName
> - 其次是 @DisplayNameGeneration
> - 再次是 junit-platform.properties 默认设置
> - 最后是默认的 Standard 类型
> 
> 也就是说，当存在多种方式时，@DisplayName 的优先级是最高的，也只会显示 @DisplayName 定义的名称


> 关于在 junit-platform.properties 配置文件中设置默认的生成器
> 如果是 Junit5 提供的生成器，配置内容为：
> 
>`junit.jupiter.displayname.generator.default = org.junit.jupiter.api.DisplayNameGenerator$ReplaceUnderscores（或其他几个默认的生成器）`
> 
> 如果是自定义的生成器，配置内容中，要填写生成器这个类的完全限定类名：
> 
> `junit.jupiter.displayname.generator.default = 包名.类名`
> 

# 3. 断言

junit5 的断言都是 org.junit.jupiter.api.Assertions 这个类中的静态方法。

导入时，可以使用静态导入 import static org.junit.jupiter.api.Assertions.*;

常用有以下断言：

| 方法 |                     描述                     |
| :---: |:------------------------------------------:|
| assertAll |          可以同时创建多个断言，还可以在断言内部添加代码块          |
|assertEquals|              断言预期和实际的两个对象是否相等              |
| assertNotEquals |             断言预期和实际的两个对象是否不相等              |
| assertArrayEquals |              断言预期和实际的两个数组是否相等              | 
| assertDoesNotThrow |          断言 executable 对象不会产生任何异常          | 
| assertInstanceOf |            断言对象属于某个类的实例或该类的子类实例            |
| assertIterableEquals |         断言预期和实际的 Iterable 对象是否完全相等         |
| assertLinesMatch |             断言预期和实际的字符串列表是否相等              | 
| assertNull |                断言对象是否为 null                |
| assertNotNull |               断言对象是否不为 null                |
| assertSame |           断言预期和实际的两个对象是否引用同一个对象            |
| assertNotSame |           断言预期和实际的两个对象是否不引用同一个对象           |
| assertThrows |        断言 executable 对象是否会抛出预期的异常，并且此方法还会返回这个异常         |
| assertThrowsExactly | 断言 executable 对象是否会抛出特定的某个异常，并且此方法还会返回这个异常 |
| assertTimeout |     断言 executable 对象是否会在给定的超时时间之前执行完毕      |
| assertTimeoutPreemptively |     断言 executable 对象是否会在给定的超时时间之前执行完毕      |
| assertTrue |              断言表达式结果是否为 true               |
| assertFalse |              断言表达式结果是否为 false              |
| fail |                   使测试失败                    |


## 3.1 executable 对象

上面有几个断言中提到了 executable 对象，Executable 是一个接口，一个函数式接口。

它的部分源码如下：
```
@FunctionalInterface
@API(
    status = Status.STABLE,
    since = "5.0"
)
public interface Executable {
    void execute() throws Throwable;
}
```

如果我们使用 assertAll 断言，这个断言的参数可以是 Executable 实现类的对象，所以我们可以使用匿名内部类实现 Executable 接口，并将这个匿名内部类作为参数传入这个断言方法，例如：
```
@Test
    void case1() {
        Person p = new Person("张三");
        assertAll(new Executable() {
            @Override
            public void execute() throws Throwable {
                assertTrue(1 == 1);
            }
        });
    }
```
上面的代码中，assertAll 里面的参数就是一个匿名内部类，这个匿名内部类实现了 Executable 接口，因为实现接口要实现接口里的抽象方法，
所以我们要实现 execute() 方法，方法里面有另一个断言 assertTrue。

因为方法体只有一行代码，且方法没有参数，所以我们可以使用 lambda 表达式来简写这个匿名内部类，如下：

```
assertAll(() -> assertTrue(1 == 1));
```

所以最终推导出了 Junit5 官方文档给出的示例中的写法：https://junit.org/junit5/docs/current/user-guide/#writing-tests-assertions



## 3.2 assertAll

assertAll 有多个重载方法。

可以在 assertAll 里传入多个 lambda  表达式作为参数，lambda 的方法体可以是另外的断言方法，代码实例：
也可以称这种方式为分组断言。

分组断言中的所有断言都会执行，某条断言失败不会影响其他断言的执行。
```
public class Demo {
    @Test
    void case1() {
        assertAll("分组断言",
                () -> assertEquals(1, 1, "第一个断言失败"),
                () -> assertNotEquals(2, 2, "第二个断言失败")
                );
    }

}
```

另外，还可以在 lambda 表达式中添加代码块，在代码块中我们可以写除了断言语句之外的代码：

```
class Demo {
    @Test
    void case1() {
        assertAll("包含代码块",
                () -> {
                    int age = 20;
                    assertTrue(age > 18, "年龄小于 18 ，断言失败");
                },
                () -> {
                    int age = 30;
                    assertTrue(age < 35, "年龄大于 35 ，断言失败");
                }
        );
    }

}
```


## 3.3 assertEquals

assertEquals 断言两个对象是否相等，它的源码使用的是 Objct 类的 equals 方法去进行比较，
因为Objct 类的 equals 方法等价于 == ，对于引用类型而言，比较的是地址值，如果想要比较引用类型数据的内容是否相等，就要重写 Object 类的 equals 方法。

java 里面有一些重写了 equals 方法的类，比如 String。

如果我们断言的是我们自己定义的类对象，当这个自定义的类没有重写 equals 方法时，断言的结果将是 false，重写了 equals 方法之后才是 true。

代码示例：
```
public class Demo {
    // 测试方法1，比较两个字符串是否相等
    // 两个字符串对象的地址值是不同的，但是 String 类重写了 equals 方法，比较的是字符串内容是否相等
    // 所以 assertEquals 断言比较的是两个字符串内容是否相等
    @Test
    void case1() {
        String s1 = "abc";
        String s2 = new String("abc");
        assertEquals(s1, s1);

    }

    // 测试方法2，比较两个Student 类对象是否相等
    // 此时 Student 类还没有重写 equals 方法，所以比较的是地址值
    // 但是这两个对象的地址值不同，所以结果是 false
    @Test
    void case2() {
        Student s1 = new Student("张三");
        Student s2 = new Student("张三");
        assertEquals(s1, s2); // false
    }
    
    /* 
    * 断言结果
    * org.opentest4j.AssertionFailedError: 
    * Expected :study.Student@2698dc7
    * Actual   :study.Student@43d7741f
    */

}
```

重写 Student 类的 equals 方法：
```
public class Student {
    private String name;
    Student() {};

    public Student(String name) {
        this.name = name;
    }

    // 重写的 equals 方法，可以使用 idea 自动生成
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return Objects.equals(name, student.name);
    }
    
}
```

重写之后比较的就是内容， 具体比的是对象的属性值，例如上面代码中的 name 的值是否相等。



## 3.4 assertNotEquals

断言两个对象是否不相等。

```
class Demo {
    @Test
    void case1() {
        String s1 = "abc";
        String s2 = "abb";
        assertNotEquals(s1, s2);  // 断言通过

    }

    @Test
    void case2() {
        String s1 = "aaa";
        String s2 = "aaa";
        assertNotEquals(s1, s2);  // s1和s2 是相等的，所以这个断言会失败
    }

}
```


## 3.5 assertArrayEquals

断言预期和实际的两个数组是否相等。

这个方法底层是通过遍历来对比两个数组中的元素是否相等。

```
class Demo {
    @Test
    void case1() {
        int[] arr1 = {1, 2, 3};
        int[] arr2 = arr1;
        assertArrayEquals(arr1, arr2);  // 断言通过

    }

    @Test
    void case2() {
        char[] c1 = {'a', 'b', 'c'};
        char[] c2 = {'a', 'b'};
        assertArrayEquals(c1, c2);  // 断言不通过
    }

}
```


## 3.6 assertDoesNotThrow

断言代码不会抛出任何异常。

我们可以传入 lambda 表达式。

```
class Demo {
    @Test
    void case1() {
        assertDoesNotThrow(() -> 1/1);  // 断言通过
    }

    @Test
    void case2() {
        assertDoesNotThrow(() -> 1/0);  // 断言不通过，因为 1/0 会产生异常 java.lang.ArithmeticException: / by zero
    }

}
```


## 3.7 assertInstanceOf

断言对象属于某个类的实例或某个类的子类实例。

创建两个普通类，一个叫 Person，一个叫 Student。Student 类继承自 Person 类。
```
// 1、 Person 类
public class Person {
    private String name;


    public Person() {
    }

    public Person(String name) {
        this.name = name;
    }

    /**
     * 获取
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     * @param name
     */
    public void setName(String name) {
        this.name = name;
    }

    public String toString() {
        return "Person{name = " + name + "}";
    }
}

// 2、Student 类
public class Student extends Person {
    private String name;


    public Student() {
    }

    public Student(String name) {
        this.name = name;
    }

    /**
     * 获取
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     * @param name
     */
    public void setName(String name) {
        this.name = name;
    }

    public String toString() {
        return "Student{name = " + name + "}";
    }
}
```

创建测试类，测试类中创建两个测试方法，一个测试方法中实例化 Person 类的对象，拿这个 Person 对象和 Person类作为参数传入断言方法中。
另一个测试方法中实例化 Student 类的对象，拿这个 Student 类的对象和 Person 类作为参数传入断言方法。

```
class Demo {
    @Test
    void case1() {
        Person p = new Person("张三");
        assertInstanceOf(Person.class, p, "父类的对象和父类本身");
    }

    @Test
    void case2() {
        Student s = new Student("学生李四");
        assertInstanceOf(Student.class, s, "子类对象和父类");
    }

    @Test
    void case3() {
        Person p = new Student("学生王二");
        assertInstanceOf(Student.class, p, "父类的引用执行子类的对象");
    }

}
```


## 3.8 assertIterableEquals

断言 Iterable 接口的实现类对象是否相等，两个对象中的元素数量和顺序必须相等，迭代时相同顺序对应的元素必须相等。

实现了 Iterable 接口的类有 ArrayList、LinkedList 等。



实现官方文档提供的例子来验证这个断言：
```
class Demo {
    @Test
    void case1() {
        Iterable<Integer> i0 = new ArrayList<>(asList(1, 2, 3));
        Iterable<Integer> i1 = new LinkedList<>(asList(1, 2, 3));

        assertIterableEquals(i0, i1);  // true
    }

    @Test
    void case2() {
        Iterable<Integer> i0 = new ArrayList<>(asList(1, 2, 3));
        Iterable<Integer> i1 = new LinkedList<>(asList(1, 2, 4));
        assertIterableEquals(i0, i1);  // 元素不相等，false
    }

}
```


## 3.9 assertLinesMatch

这个断言可以检查两个字符串集合中，相同顺序的字符串是否相等。

首先会检查两个集合的长度是否相等，然后会检查两个集合在相同位置上的元素是否相等。

```
class Demo {
    @Test
    void case1() {
        Iterable<String> s0 = new ArrayList<>(asList("abc", "def", "ghi"));
        Iterable<String> s1 = new LinkedList<>(asList("abc", "def", "ghi"));

        assertIterableEquals(s0, s1);  // true
    }

    @Test
    void case2() {
        Iterable<String> s0 = new ArrayList<>(asList("abc", "def", "ghi"));
        Iterable<String> s1 = new LinkedList<>(asList("abc", "def", "ghii"));
        assertIterableEquals(s0, s1);  // 两个字符串不相等，false
    }

}
```

## 3.10 assertNull

断言对象是否为 null

```
public class Demo {
    @Test
    void case1() {
        String s = null;
        assertNull(s, "对象不为 null"); // 断言通过
    }

    @Test
    void case2() {
        String s = "aaa";
        assertNull(s, "对象不为 null");  // 断言失败
    }

}
```


## 3.11 assertNotNull

断言是否不为 null

```
public class Demo {
    @Test
    void case1() {
        String s = null;
        assertNotNull(s, "对象为 null"); // 断言失败
    }

    @Test
    void case2() {
        String s = "aaa";
        assertNotNull(s, "对象为 null"); // 断言通过
    }

}
```


## 3.12 assertSame 和 assertNotSame

assertSame 断言预期和实际的两个对象是否引用同一个对象，通过比较地址值来判断。

assertSame 断言预期和实际的两个对象不是引用自同一个对象，通过比较地址值来判断。

```
public class Demo {
    @Test
    void case1() {
        String s = "abc";
        String s1 = s;
        String s2 = s;
        assertSame(s1, s2); // 断言通过
    }

    @Test
    void case2() {
        String s = "abc";
        String s1 = s;
        String s2 = new String("abc");
        assertSame(s1, s2); // 断言失败
    }

}
```


## 3.13 assertThrows

断言 Executable 接口的实现类对象是否会抛出指定的异常。

如果没有抛出异常，或者没有抛出指定的异常，断言都会失败。

断言参数可以传入异常类对象和 Executable 接口的实现类对象。

```
public class Demo {
    @Test
    void case1() {
        // 断言第二个参数匿名内部类会产生 ArrayIndexOutOfBoundsException 数组下标越界异常
        // 但是我们第一个参数预期的异常是 ArithmeticException 运算条件异常，所以这个断言会失败
        // 因为这个断言方法有返回值，我们可以定义一个变量来接收返回值， 返回值就是产生的异常
        // 因为我们不确定返回什么异常，所以我们用 Exception 这个父类来定义对象的类型
        Exception e =  assertThrows(ArithmeticException.class, () -> {
            int[] arr = {1};
            System.out.println(arr[2]);
        });
    }

    @Test
    void case2() {
        // 因为断言失败时后面的代码不会执行，所以这条测试用例断言会成功
        // 成功之后执行后面的异常对象信息打印的方法
        Exception e =  assertThrows(ArrayIndexOutOfBoundsException.class, () -> {
            int[] arr = {1};
            System.out.println(arr[2]);
        });

        System.out.println("获取异常的信息" + e);  // 输出：获取异常的信息java.lang.ArrayIndexOutOfBoundsException: 2
    }

}
```

## 3.14 assertThrowsExactly

断言 Executable 接口的实现类对象是否会抛出指定的异常。

如果没有抛出异常，或者没有抛出指定的异常，断言都会失败。

在 junit5.8 之后可以使用这个断言方法，这个方法和 assertThrows 的区别暂时没有找到相关的资料。


## 3.15 assertTimeout

断言 Executable 接口的实现类对象在给定的时间内是否会抛出指定的异常。

可以理解为断言代码执行是否超时。

```
public class Demo {
    @Test
    void case1() {
        // 给定时间 3 秒，表示 3 秒内要执行完毕
        // lambda 表达式中的代码线程等待 1 秒
        // 然后执行打印语句
        // 整体时间没有超过 3 秒
        // 断言将通过
        assertTimeout(Duration.ofSeconds(3), () -> {
            Thread.sleep(1000);
            System.out.println("等待一秒后执行");
        });
    }

    @Test
    void case2() {
        // 给定时间 3 秒，表示 3 秒内要执行完毕
        // lambda 表达式中的代码线程等待 4 秒
        // 然后执行打印语句
        // 整体时间超过 3 秒
        // 断言将失败
        // 代码将抛出异常 org.opentest4j.AssertionFailedError: execution exceeded timeout of 3000 ms by 1003 ms
        assertTimeout(Duration.ofSeconds(3), () -> {
            Thread.sleep(4000);
            System.out.println("等待4秒后执行");
        });
    }

}
```


## 3.16 assertTimeoutPreemptively

和 assertTimeout 作用相同，但是它们的内部实现方式有区别。

使用 assertTimeout 方法，即便超时，它也会让代码继续执行完毕。

assertTimeoutPreemptively 方法，一旦超时，那么就会立即停止执行，即便还有代码没执行完，也会立即终止。



## 3.17 assertTrue 和 assertFalse

断言表达式结果是否为 true 或 false

```
public class Demo {
    @Test
    void case1() {
        assertTrue(1 < 2);
    }

    @Test
    void case2() {
        assertFalse(1 < 0);
    }

}
```

## 3.18 fail

fail 方法会直接让测试方法失败，并结束测试方法的执行。

```
public class Demo {
    @Test
    void case1() {
        // 当 i = 1 时，直接让测试失败，并终止代码执行
        for (int i = 0; i < 3; i++) {
            String step = String.format("执行测试用例第 %d 个步骤", i);
            System.out.println(step);
            if (i == 1) fail("测试失败");
        }
    }
}
```



##  3.19 第三方断言库

如AssertJ、Hamcrest、Truth等


# 4. 假设

假设方法都是 `org.junit.jupiter.api.Assumptions` 类中的静态方法。

假设有点类似于断言。

junit 提供的假设方法有：

| 方法 |                描述                |
| :---: |:--------------------------------:|
| abort() |               中止测试               |
| abort(String message) |      中止测试，并返回指定的 message 内容      |
| assumeFalse(boolean assumption) |         验证给定的假设表达式是否不成立          | 
| assumeFalse(boolean assumption, String message) |   验证给定的假设表达式，并返回指定的 message 内容   |
| assumeTrue(boolean assumption) |          验证给定的假设表达式是否成立          |
| assumeTrue(boolean assumption, String message) | 验证给定的假设表达式是否成立，并返回指定的 message 内容 |
| assumingThat(boolean assumption, Executable executable) | 当假设成立，则执行 Executable 函数式接口的实现对象  |


## 4.1 abort()

此方法可用于中止测试，代码示例：
```
@Test
void test_case1() {
    System.out.println("你好");
    abort();
    System.out.println("我将不会被执行");
}
```

此方法会抛出 `TestAbortedException` 异常。


## 4.2 abort(String message)

此方法可用于中止测试，并且输出传给 message 参数的内容，代码示例：
```
@Test
void test_case1() {
    System.out.println("你好");
    abort("中止执行");
    System.out.println("我将不会被执行");
}
```
idea 中运行之后，输出如下：
[![p9ZPOk4.png](https://s1.ax1x.com/2023/04/22/p9ZPOk4.png)](https://imgse.com/i/p9ZPOk4)


## 4.3 assumeFalse(boolean assumption) 和 assumeFalse(boolean assumption, String message)

此方法用于验证给定的假设表达式是否不成立。

如果假设不成立，则继续执行此方法之后的代码。

如果假设成立，则不会执行后面的代码。

通俗地说，assumeFalse 方法，就是验证某个表达式的返回值是不是 False。

是 False，则继续执行后面的代码，不是 False，则中止执行所在的测试方法，但不会中止整个测试。


类似 assertFalse。

代码示例：
```
@Test
void test_case1() {
    System.out.println("你好1");
    assumeFalse(1 > 2);  // 因为 1 > 2 结果是 False，所以 assumeFalse 假设成立，后面的 aaa 会输出
    System.out.println("aaa");
}

@Test
void test_case2() {
    System.out.println("你好2");
    assumeFalse(1 < 2, "假设不成立"); // 因为 1 < 2 结果是 True，所以 assumeFalse 假设不成立，后面的 aaa 不会输出
    System.out.println("aaa");
}
```

idea 中执行之后的结果：
[![p9ZiTCd.png](https://s1.ax1x.com/2023/04/22/p9ZiTCd.png)](https://imgse.com/i/p9ZiTCd)


## 4.4 assumeTrue(boolean assumption) 和 assumeTrue(boolean assumption, String message)

此方法用于验证给定的表达式是否成立。类似 assertTrue。

通俗地说，assertTrue 方法，就是验证某个表达式的返回值是不是 True。

是 True，则继续执行后面的代码，不是 True，则中止执行所在的测试方法，但不会中止整个测试。

代码示例：
```
@Test
void test_case1() {
    System.out.println("你好1");
    assumeTrue(1 > 2);
    System.out.println("aaa");
}

@Test
void test_case2() {
    System.out.println("你好2");
    assumeTrue(1 < 2, "假设成立");
    System.out.println("aaa");
}
```

idea 中执行之后的结果：
[![p9ZFBsP.png](https://s1.ax1x.com/2023/04/22/p9ZFBsP.png)](https://imgse.com/i/p9ZFBsP)


## assumingThat(boolean assumption, Executable executable)

此方法用于验证表达式是否成立，成立则执行 Executable 函数式接口的实现对象。

成立和不成立都不会影响此方法后面的代码执行，更不会影响整个测试过程。

代码示例：
```
@Test
    void test_case1() {
        System.out.println("你好1");
        assumingThat(false, () -> System.out.println("哈哈哈"));
        System.out.println("aaa");
    }

    @Test
    void test_case2() {
        System.out.println("你好2");
        assumingThat(true, () -> System.out.println("哈哈哈"));
        System.out.println("aaa");
    }
```

idea 中执行结果如下：
[![p9Zkfte.png](https://s1.ax1x.com/2023/04/22/p9Zkfte.png)](https://imgse.com/i/p9Zkfte)



# 5. 注解
## 5.1 禁用测试 @Disabled

此注解用于禁用测试类或测试方法，即被注解的类或方法不会执行。

- 禁用测试方法
```
public class TestDemo {

    @BeforeAll
    static void setup() {
        System.out.println("初始化测试环境");
    }

    @Test
    void testCase1() {
        System.out.println("执行用例1");
        assertEquals(1+1, 2);
    }

    @Test
    @Disabled  // testCase2 被禁用，不会执行
    void testCase2() {
        System.out.println("执行用例2 —— 被禁用，不会执行");
        assertEquals(2+2, 4);
    }

    @AfterAll
    static void teardown() {
        System.out.println("清理测试环境");
    }
}
```

执行结果：
```
...
初始化测试环境
执行用例1
清理测试环境
...
```

- 禁用测试类
```
// 禁用测试类 —— TestDemo
@Disabled
public class TestDemo {

    @BeforeAll
    static void setup() {
        System.out.println("初始化测试环境");
    }

    @Test
    void testCase1() {
        System.out.println("执行用例1");
        assertEquals(1+1, 2);
    }

    @Test
    void testCase2() {
        System.out.println("执行用例2 —— 被禁用，不会执行");
        assertEquals(2+2, 4);
    }

    @AfterAll
    static void teardown() {
        System.out.println("清理测试环境");
    }
}


// 测试类 TestDemo2 未被禁用
class TestDemo2 {
    @Test
    void testcase3() {
        System.out.println("测试用例3");
    }
}
```

执行结果：
```
...
测试用例3
...
```

被禁用的测试类或测试方法，在执行的时候，都会被当成跳过执行的用例，Skipped。

```Tests run: 2, Failures: 0, Errors: 0, Skipped: 1```


## 5.2 满足条件时执行

- `@EnabledOnOs(MAC)`  -- 如果当前系统为 mac 系统，则执行
- `@DisabledOnOs({ LINUX, WINDOWS })` -- 如果当前系统为 windows 或 linux，则不执行
- `@EnabledOnJre(JAVA_8)` -- 如果 jre 版本为 1.8 则执行
- `@EnabledForJreRange(min = JAVA_9, max = JAVA_11)` -- 指定 jre 版本范围，在指定的范围内才执行
- 自定义的条件 -- 当满足自定的条件时才会执行/不执行


## 5.3 自定义条件来控制测试用例执行或不执行

第一种方式，在同一个测试类中定义用于条件的方法，测试用例是否执行，会根据条件方法的返回值来确定。

如下面的代码中 `customCondition1` 和 `customCondition2` 就是我们定义的条件方法，如果条件方法读起来拗口，可以直接就把它们当成条件。

然后使用 `@EnabledIf()` 注解来进行条件的限制，将我们定义好的条件名称(方法名称)传给这个注解，但是要用`字符串`的形式。

如果条件的返回值为 true，就会执行被注解的测试方法，也就是条件为 true 时才执行测试用例。

如果是 false，则不执行。

```
public class TestDemo {
    @Test
    @EnabledIf("customCondition1")   // 在这个测试方法上使用自定义的条件 customCondition1，这个测试方法将会正确执行
    void test_case1() {
        System.out.println("测试用例1");
    }


    @Test
    @EnabledIf("customCondition2")  // 在这个测试方法上使用自定义的条件 customCondition2，这个测试方法将不会执行
    void test_case2() {
        System.out.println("测试用例2");
    }

    // 这个方法是自定义的条件，返回 true，模拟条件成立
    boolean customCondition1() {
        return true;
    }

    // 这个方法是自定义的条件，返回 false，模拟条件不成立
    boolean customCondition2() {
        return false;
    }
}
```
上面的代码在 idea 中的执行结果如下：
[![p9ZVHn1.png](https://s1.ax1x.com/2023/04/22/p9ZVHn1.png)](https://imgse.com/i/p9ZVHn1)



第二种方式，条件方法可以位于测试类(不同类、不同包)之外，在使用时，需要用完全限定名称来引用条件方法，代码示例如下：

```
// 1.条件方法所在的类
package study;

public class CustomCondition {
    
    // 条件方法1，返回 true
    static boolean myCondition1() {
        return true;
    }

    // 条件方法2，返回 false
    static boolean myCondition2() {
        return false;
    }
}



// 2.测试方法所在的类
public class TestDemo {
    @Test
    @EnabledIf("study.CustomCondition#myCondition1")  // 引用另一个 java 文件中的条件方法，从包名一直写到类名
    void test_case1() {
        System.out.println("测试用例1");
    }


    @Test
    @EnabledIf("study.CustomCondition#myCondition2") // 引用另一个 java 文件中的条件方法，从包名一直写到类名
    void test_case2() {
        System.out.println("测试用例2");
    }
}

```

代码执行之后的结果如下：
[![p9ZZbvj.png](https://s1.ax1x.com/2023/04/22/p9ZZbvj.png)](https://imgse.com/i/p9ZZbvj)


## 5.4 关于自定义条件的总结

- 条件方法必须是 `static` 方法
- 使用时将条件方法的名称传给 `@EnabledIf()` 和 `@DisabledIf()` 注解
- 条件方法可以在测试方法内，也可以定义在其他地方，如果定义在测试类之外的其他地方，需要使用完全限定名称来引用
