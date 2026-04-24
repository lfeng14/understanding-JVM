- 假设要编译大版本号为N的JDK，我们还要另外准备一个大版本号至少为N-1的、已经编译
好的JDK，这是因为OpenJDK由多个部分（HotSpot、JDK类库、JAXWS、JAXP……）构成，其中一
部分（HotSpot）代码使用C、C++编写，而更多的代码则是使用Java语言来实现，因此编译这些Java代
码就需要用到另一个编译期可用的JDK，官方称这个JDK为“Bootstrap JDK”
。编译OpenJDK 12时，Bootstrap JDK必须使用JDK 11及之后的版本。在Ubuntu中使用以下命令安装OpenJDK 11：
  ```
  sudo apt-get install openjdk-11-jdk
  ```

  <img width="700" height="420" alt="image" src="https://github.com/user-attachments/assets/43d8547e-99bd-4e5b-8660-f78d12c374f5" />

- Java ME（Micro Edition）：支持Java程序运行在移动终端（手机、PDA）上的平台，对Java API
有所精简，并加入了移动终端的针对性支持，这条产品线在JDK 6以前被称为J2ME。有一点读者请勿
混淆，现在在智能手机上非常流行的、主要使用Java语言开发程序的Android并不属于Java ME。
- Java SE（Standard Edition）：支持面向桌面级应用（如Windows下的应用程序）的Java平台，提
供了完整的Java核心API，这条产品线在JDK 6以前被称为J2SE。
- Java EE（Enterprise Edition）：支持使用多层架构的企业应用（如ERP、MIS、CRM应用）的
Java平台，除了提供Java SE API外，还对其做了大量有针对性的扩充[4]
，并提供了相关的部署支持，这条产品线在JDK 6以前被称为J2EE，在JDK 10以后被Oracle放弃，捐献给Eclipse基金会管理，此后被
称为Jakarta EE。

- 构建
  ```
  A new configuration has been successfully created in /home/icyfenix/develop/java/jdk12/build/linux-x86
  _
  using default settings.
  64-server-release
  Configuration summary:
  * Debug level: release
  * HS debug level: product
  * JVM variants: server
  * JVM features: server: 'aot cds cmsgc compiler1 compiler2 epsilongc g1gc graal jfr jni-check jvmci jvmti mana
  * OpenJDK target: OS: linux, CPU architecture: x86, address length: 64
  * Version string: 12-internal+0-adhoc.icyfenix.jdk12 (12-internal)
  Tools summary:
  * Boot JDK: * Toolchain: * C Compiler: * C++ Compiler: openjdk version "11.0.3" 2019-04-16 OpenJDK Runtime Environment (build 11.0.3+7-Ubuntu-1ubuntu
  gcc (GNU Compiler Collection)
  Version 7.4.0 (at /usr/bin/gcc)
  Version 7.4.0 (at /usr/bin/g++)
  Build performance summary:
  * Cores to use: 4
  * Memory limit: 7976 MB
  ```
- 在configure命令以及后面的make命令的执行过程中，会在“build/配置名称”目录下产生如下目录结
构。不常使用C/C++的读者要特别注意，如果多次编译，或者目录结构成功产生后又再次修改了配
置，必须先使用“make clean”和“make dist-clean”命令清理目录，才能确保新的配置生效。编译产生的目
录结构以及用途如下所示：
  ```
  buildtools/：用于生成、存放编译过程中用到的工具
  hotspot/：HotSpot虚拟机编译的中间文件
  images/：使用make *-image产生的镜像存放在这里
  jdk/：编译后产生的JDK就放在这里
  support/：存放编译时产生的中间文件
  test-results/：存放编译后的自动化测试结果
  configure-support/：这三个目录是存放执行configure、make和test的临时文件
  make-support/
  test-support/
  ```
- 依赖检查通过后便可以输入“make images”执行整个OpenJDK编译了，这里“images”是“product-
images”编译目标（Target）的简写别名，这个目标的作用是编译出整个JDK镜像，除了“product-
images”以外，其他编译目标还有：
  ```
  hotspot：只编译HotSpot虚拟机
  hotspot-<variant>：只编译特定模式的HotSpot虚拟机
  docs-image：产生JDK的文档镜像
  test-image：产生JDK的测试镜像
  all-images：相当于连续调用product、docs、test三个编译目标
  bootcycle-images：编译两次JDK，其中第二次使用第一次的编译结果作为Bootstrap JDK
  clean：清理make命令产生的临时文件
  dist-clean：清理make和configure命令产生的临时文件
  ```
- 使用Oracle VM V irtualBox虚拟机，启动4条编译线程，8GB内存，全量编译整个OpenJDK 12
大概需近15分钟时间，如果之前已经全量编译过，只是修改了少量文件的话，增量编译可以在数十秒
内完成。编译完成之后，进入OpenJDK源码的“build/配置名称/jdk”目录下就可以看到OpenJDK的完整
编译结果了，把它复制到JAVA_HOME目录，就可以作为一个完整的JDK来使用，如果没有人为设置
过JDK开发版本的话，这个JDK的开发版本号里默认会带上编译的机器名，如下所示：
  ```
  > ./java -version
  openjdk version "12-internal" 2019-03-19
  OpenJDK Runtime Environment (build 12-internal+0-adhoc.icyfenix.jdk12)
  OpenJDK 64-Bit Server VM (build 12-internal+0-adhoc.icyfenix.jdk12, mixed mode)
  ```
- 编译jdk和使用clion调试jdk
  <img width="650" height="444" alt="image" src="https://github.com/user-attachments/assets/296d89d2-b4e8-421b-acfd-c39f407c0f5b" />

#### further reading
- https://juejin.cn/post/6982024331467423774
