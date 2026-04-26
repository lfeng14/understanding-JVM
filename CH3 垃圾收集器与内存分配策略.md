- 可达性分析，gc-root是多叉树；
<img width="300" height="230" alt="image" src="https://github.com/user-attachments/assets/92ccb17f-0287-4e16-a847-72be14353c7f" />

  - 虚拟机栈（栈帧中的本地变量表）中引用的对象
    - **具体包括**：当前正在执行的方法里的参数、局部变量、临时变量等。
    - **原理**：这些变量直接持有对象引用，是程序当前正在使用的对象，显然不能回收。
    - **例子**：`void foo(Object p) { Object o = new Object(); ... }` 中的 `p` 和 `o` 就是栈上的引用。
    
  - 方法区中类静态属性引用的对象
    - **具体**：`static` 修饰的字段所引用的对象。
    - **原理**：静态字段属于类，只要类未被卸载，静态字段引用的对象就应该存活。
    - **例子**：`private static List<String> cache = new ArrayList<>();` 中的 `cache` 对象。
    
  - 方法区中常量引用的对象
    - **具体**：字符串常量池（String Table）中的引用，以及 `final static` 常量。
    - **原理**：常量通常不会被改变，且可能在编译期就被确定，必须保证它们始终可用。
    - **例子**：`public static final String MSG = "Hello";` 中的 `"Hello"` 字符串对象。
    
  - 本地方法栈中 JNI（Native方法）引用的对象
    - **具体**：通过 JNI 调用 `NewGlobalRef`、`NewLocalRef` 等创建的引用，或者 native 代码中持有的 Java 对象。
    - **原理**：JVM 无法控制 native 代码内部逻辑，但 native 代码可能正在使用这些对象，因此它们必须作为根。
    - **例子**：一个 C 函数通过 JNI 获取了 `jobject` 并暂存于全局变量，这个对象就不能被回收。
    
  - Java虚拟机内部的引用
    - **具体**：基本数据类型对应的 `Class` 对象（如 `int.class`）、一些常驻异常（如 `NullPointerException`、`OutOfMemoryError` 的实例）、系统类加载器等。
    - **原理**：这些对象是 JVM 运行的基础设施，只要 JVM 还在运行，它们就应该一直存活。
    - **例子**：`java.lang.Object` 的 `Class` 实例，以及 `ClassLoader.getSystemClassLoader()` 返回的类加载器。
    
  - 所有被同步锁（synchronized）持有的对象
    - **具体**：被 `synchronized` 修饰的代码块或方法锁定的那个对象（即 `monitor`）。
    - **原理**：如果对象正在作为锁，回收它会导致同步逻辑出错；需要保证锁对象在锁释放之前不被回收。
    - **例子**：`synchronized (lockObj) { ... }` 中的 `lockObj`。
    
  - 反映Java虚拟机内部情况的JMXBean、JVMTI回调、本地代码缓存等
    - **具体**：JMX 注册的 MBean、通过 JVMTI（JVM Tool Interface）注册的回调函数所引用的对象、JIT 编译后生成的本地代码缓存等。
    - **原理**：这些属于 JVM 自身的运行时数据或扩展工具所需的引用，用于监控、调试或性能优化，必须保持存活。
    - **例子**：通过 `ManagementFactory.getMemoryMXBean()` 获取的 `MemoryMXBean` 对象，或者 JVMTI agent 附加时引用的某些类。
- 不可达时，这种自救的机会只有一次，因为一个对象的finalize()方法最多只会被系统自动调用一次
