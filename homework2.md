# 第二次作业

## Why should you have minimum scope for variables

1. 可以增强代码的可读性和可维护性，降低出错的可能
2. 过早地声明局部变量可能导致其作用域不仅过早开始而且结束太晚。 局部变量的作用域从声明它的位置延伸到封闭块的末尾。 如果变量在使用它的封闭块之外声明，则在程序退出该封闭块后它仍然可见。如果在其预定用途区域之前或之后意外使用变量，则后果可能是灾难性的。
3. 最小化局部变量作用域的最终技术是保持方法小而集中。如果在同一方法中组合两个activities，则与一个行为相关的局部变量可能会位于执行另一个行为的代码范围内。 为了防止这种情况发生，只需将方法分为两个：每个行为对应一个方法。

> effictive java 第三版 57. 最小化局部变量的作用域
<https://sjsdfg.github.io/effective-java-3rd-chinese/#/README>

## Why should you understand performance of String Concatenation

1. 字符串连接操作符(+)是将几个字符串组合成一个字符串的简便方法。对于生成单行输出或构造一个小的、固定大小的对象的字符串表示形式，它是可以的，但是它不能伸缩。使用字符串串联运算符重复串联n个字符串需要n的平方级时间。这是字符串不可变这一事实导致的结果。当连接两个字符串时，将复制这两个字符串的内容。
2. 如果项的数量很大，则该方法的性能非常糟糕。要获得能接受的性能，请使用StringBuilder代替String来存储正在构建的语句
3. 不要使用字符串连接操作符合并多个字符串，除非性能无关紧要。否则使用 StringBuilder 的 append 方法。或者，使用字符数组，再或者一次只处理一个字符串，而不是组合它们。

> effictive java 第三版 63. 当心字符串连接引起的性能问题
<https://sjsdfg.github.io/effective-java-3rd-chinese/#/README>

## What are the best practices with Exception Handling

1. Never hide Exception. At least log them
2. Do not use Exception handling for Flow control. They have significant performance impact
3. Think about user to give error message and think about support developer what information to log
4. Try to make the custom exception as Unchecked exception
5. Have Global exception handling
6. Try to avoid exception handling at lower level of application.

<https://github.com/deepak1410/core_java/wiki/Exception-Handlling>

## When is it recommended to prefer Unchecked Exceptions

Checked Exceptions are great, so long as you understand when they should be used. The Java core API fails to follow these rules for SQLException (and sometimes for IOException) which is why they are so terrible.

Checked Exceptions should be used for predictable, but unpreventable errors that are reasonable to recover from.

Unchecked Exceptions should be used for everything else.

I'll break this down for you, because most people misunderstand what this means.

Predictable but unpreventable: The caller did everything within their power to validate the input parameters, but some condition outside their control has caused the operation to fail. For example, you try reading a file but someone deletes it between the time you check if it exists and the time the read operation begins. By declaring a checked exception, you are telling the caller to anticipate this failure.
Reasonable to recover from: There is no point telling callers to anticipate exceptions that they cannot recover from. If a user attempts to read from an non-existing file, the caller can prompt them for a new filename. On the other hand, if the method fails due to a programming bug (invalid method arguments or buggy method implementation) there is nothing the application can do to fix the problem in mid-execution. The best it can do is log the problem and wait for the developer to fix it at a later time.
Unless the exception you are throwing meets all of the above conditions it should use an Unchecked Exception.

Reevaluate at every level: Sometimes the method catching the checked exception isn't the right place to handle the error. In that case, consider what is reasonable for your own callers. If the exception is predictable, unpreventable and reasonable for them to recover from then you should throw a checked exception yourself. If not, you should wrap the exception in an unchecked exception. If you follow this rule you will find yourself converting checked exceptions to unchecked exceptions and vice versa depending on what layer you are in.

For both checked and unchecked exceptions, use the right abstraction level. For example, a code repository with two different implementations (database and filesystem) should avoid exposing implementation-specific details by throwing SQLException or IOException. Instead, it should wrap the exception in an abstraction that spans all implementations (e.g. RepositoryException).

<https://stackoverflow.com/questions/27578/when-to-choose-checked-and-unchecked-exceptions>

## When do you use a Marker Interface

1. 标记接口（marker interface），是不包含方法声明的接口，只是指定或标记一个类实现了具有某些属性的接口。
2. 总之，标记接口和标记注释都有其用处。 如果你想定义一个没有任何关联的新方法的类型，一个标记接口是一种可行的方法。 如果要标记除类和接口以外的程序元素，或者将标记符合到已经大量使用注解类型的框架中，那么标记注解是正确的选择。 如果发现自己正在编写目标为 ElementType.TYPE 的标记注解类型，那么请花时间弄清楚究竟应该用注解类型，还是标记接口更合适。

> effictive java 第三版 41. 使用标记接口定义类型
<https://sjsdfg.github.io/effective-java-3rd-chinese/#/README>

## Why are ENUMS important for Readable Code

1. You should always use enums when a variable (especially a method parameter) can only take one out of a small set of possible values. Examples would be things like type constants (contract status: "permanent", "temp", "apprentice"), or flags ("execute now", "defer execution").

If you use enums instead of integers (or String codes), you increase compile-time checking and avoid errors from passing in invalid constants, and you document which values are legal to use.

BTW, overuse of enums might mean that your methods do too much (it's often better to have several separate methods, rather than one method that takes several flags which modify what it does), but if you have to use flags or type codes, enums are the way to go.
As an example, which is better?

<https://stackoverflow.com/questions/4709175/what-are-enums-and-why-are-they-useful>

## Why should you minimize mutability

1. 不可变类简单来说是它的实例不能被修改的类。 包含在每个实例中的所有信息在对象的生命周期中是固定的，因此不会观察到任何变化。 Java 平台类库包含许多不可变的类，包括 String 类，基本类型包装类以及BigInteger 类和 BigDecimal 类。 有很多很好的理由：不可变类比可变类更容易设计，实现和使用。 他们不太容易出错，更安全。
2. 不可变对象很简单。 一个不可变的对象可以完全处于一种状态，也就是被创建时的状态。 如果确保所有的构造方法都建立了类不变量，那么就保证这些不变量在任何时候都保持不变，使用此类的程序员无需再做额外的工作。 另一方面，可变对象可以具有任意复杂的状态空间。 如果文档没有提供由设置方法执行的状态转换的精确描述，那么可靠地使用可变类可能是困难的或不可能的。
3. 不可变对象本质上是线程安全的。 它们不需要同步。 被多个线程同时访问它们时并不会被破坏。 这是实现线程安全的最简单方法。 由于没有线程可以观察到另一个线程对不可变对象的影响，所以不可变对象可以被自由地共享。 因此，不可变类应鼓励客户端尽可能重用现有的实例。 一个简单的方法是为常用的值提供公共的静态 final 常量。
4. 不仅可以共享不可变的对象，而且可以共享内部信息。
5. 不可变对象为其他对象提供了很好的构件，无论是可变的还是不可变的。 如果知道一个复杂组件的内部对象不会发生改变，那么维护复杂对象的不变量就容易多了。这一原则的特例是，不可变对象可以构成 Map 对象的键和 Set 的元素，一旦不可变对象作为 Map 的键或 Set 里的元素，即使破坏了 Map 和 Set 的不可变性，但不用担心它们的值会发生变化。
6. 不可变对象提供了免费的原子失败机制。它们的状态永远不会改变，所以不可能出现临时的不一致。
7. 不可变类的主要缺点是对于每个不同的值都需要一个单独的对象。 创建这些对象可能代价很高，特别是如果是大型的对象下。

> effictive java 第三版 17. 最小化可变性
<https://sjsdfg.github.io/effective-java-3rd-chinese/#/README>

## What is functional programming

1. 在维基百科上，函数式编程的定义如下："函数式编程是一种编程范式。它把计算当成是数学函数的求值，从而避免改变状态和使用可变数据。它是一种声明式的编程范式，通过表达式和声明而不是语句来编程。" （见 Functional Programming）
2. 函数式编程的思想在软件开发领域由来已久。在众多的编程范式中，函数式编程虽然出现的时间很长，但是在编程范式领域和整个开发社区中的流行度一直不温不火。函数式编程有一部分狂热的支持者，在他们眼中，函数式编程思想是解决各种软件开发问题的终极方案；而另外的一部分人，则觉得函数式编程的思想并不容易理解，学习曲线较陡，上手起来也有一定的难度。大多数人更倾向于接受面向对象或是面向过程这样的编程范式。这也是造成函数式编程范式一直停留在小众阶段的原因。
3. 随着计算机硬件多核的普及，为了尽可能地利用硬件平台的能力，并发编程显得尤为重要。与传统的命令式编程范式相比，函数式编程范式由于其天然的无状态特性，在并发编程中有着独特的优势。以 Java 平台来说，相信很多开发人员都对 Java 的多线程和并发编程有所了解。可能最直观的感受是，Java 平台的多线程和并发编程并不容易掌握。这主要是因为其中所涉及的概念太多，从 Java 内存模型，到底层原语 synchronized 和 wait/notify，再到 java.util.concurrent 包中的高级同步对象。由于并发编程的复杂性，即使是经验丰富的开发人员，也很难保证多线程代码不出现错误。很多错误只在运行时的特定情况下出现，很难排错和修复。在学习如何更好的进行并发编程的同时，我们可以从另外一个角度来看待这个问题。多线程编程的问题根源在于对共享变量的并发访问。如果这样的访问并不需要存在，那么自然就不存在多线程相关的问题。在函数式编程范式中，函数中并不存在可变的状态，也就不需要对它们的访问进行控制。这就从根本上避免了多线程的问题。

<https://www.ibm.com/developerworks/cn/java/j-understanding-functional-programming-1/index.html>

## Why should you prefer Builder Pattern to build complex objects

1. 静态工厂和构造方法都有一个限制：它们不能很好地扩展到很多可选参数的情景。简而言之，这两种方式是有效的，但是当有很多参数时，很难编写客户端代码，而且很难读懂它。读者不知道这些值是什么意思，并且必须仔细地去数参数才能找到答案。一长串相同类型的参数可能会导致一些细微的 bug。如果客户端不小心写反了两个这样的参数，编译器并不会报错，但是程序在运行时会出现错误行为
2. 使用Builder模式代码很容易编写，更重要的是易于阅读。
3. builder 对构造方法的一个微小的优势是，builder 可以有多个可变参数，因为每个参数都是在它自己的方法中指定的。或者，builder 可以将传递给多个调用的参数聚合到单个属性中。
4. Builder 模式非常灵活。 单个 builder 可以重复使用来构建多个对象。 builder 的参数可以在构建方法的调用之间进行调整，以改变创建的对象。 builder 可以在创建对象时自动填充一些属性，例如每次创建对象时增加的序列号。
5. 总而言之，当设计类的构造方法或静态工厂的参数超过几个时，Builder 模式是一个不错的选择，特别是如果许多参数是可选的或相同类型的。builder模式客户端代码比使用伸缩构造方法（telescoping constructors）更容易读写，并且builder模式比 JavaBeans 更安全。

> effictive java 第三版 2. 当构造方法参数过多时使用 builder 模式
<https://sjsdfg.github.io/effective-java-3rd-chinese/#/README>

## Why should you avoid floats for Calculations

1. float 和 double 类型主要用于科学计算和工程计算。它们执行二进制浮点运算，该算法经过精心设计，能够在很大范围内快速提供精确的近似值。但是，它们不能提供准确的结果，也不应该在需要精确结果的地方使用。float 和 double 类型特别不适合进行货币计算，因为不可能将 0.1（或 10 的任意负次幂）精确地表示为 float 或 double。
2. 总之，对于任何需要精确答案的计算，不要使用 float 或 double 类型。如果希望系统来处理十进制小数点，并且不介意不使用基本类型带来的不便和成本，请使用 BigDecimal。使用 BigDecimal 的另一个好处是，它可以完全控制舍入，当执行需要舍入的操作时，可以从八种舍入模式中进行选择。如果你使用合法的舍入行为执行业务计算，这将非常方便。如果性能是最重要的，那么你不介意自己处理十进制小数点，而且数值不是太大，可以使用 int 或 long。如果数值不超过 9 位小数，可以使用 int；如果不超过 18 位，可以使用 long。如果数量可能超过 18 位，则使用 BigDecimal。

> effictive java 第三版 60. 若需要精确答案就应避免使用 float 和 double 类型
<https://sjsdfg.github.io/effective-java-3rd-chinese/#/README>

## Why should you build the riskiest high priority features first

