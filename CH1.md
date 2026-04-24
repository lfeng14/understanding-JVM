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
