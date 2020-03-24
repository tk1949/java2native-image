#### **使用 GraalVM 实现把 Java 字节码编译为本地可执行二进制文件**

> GraalVM提供了一个全面的生态系统，支持大量的语言集合包括（Java以及其他基于JVM的语言, JavaScript, Ruby, Python, R, 以及 C/C++ 还有其他基于LLVM的语言），同时还能支持在不同的部署场景下运行（包括OpenJDK、Node.js、MySQL、Oracle Database，或者其他独立场景）
>
> _注意: 由于 GrallVM 对于 Windows 为实验性支持，所以本文运行环境为 Linux_

##### **Step 1、下载 GraalVM CE**

> https://github.com/graalvm/graalvm-ce-builds/releases
>
> _注意: 本文下载 Linux 版本 [graalvm-ce-java11-linux-amd64-20.0.0.tar.gz](graalvm-ce-java11-linux-amd64-20.0.0.tar.gz), 也可以下载更高版本_

##### **Step 2、配置 Java 运行环境**

> 自行配置Java运行环境即可，不配置可在 Step 3 里面直接使用 `GraalVM/bin` 里面 **gu** 命令

##### **Step 3.1、安装 native-image**

> **gu install native-image**

```
Downloading: Component catalog from www.graalvm.org
Processing Component: Native Image
Downloading: Component native-image: Native Image  from github.com
Installing new component: Native Image (org.graalvm.native-image, version 19.3.0.2)
```

##### **Step 3.2、 _查看安装是否成功_**

> **gu list**

```
ComponentId              Version             Component name      Origin 
--------------------------------------------------------------------------------
graalvm                  19.3.0.2            GraalVM Core        
native-image             19.3.0.2            Native Image        github.com
```

> ---**_环境配置到此处结束_**---

##### **Step 4、编写 HelloWord**

```java
public class HelloWorld
{
  public static void main(String[] args)
 {
    System.out.println("Hello, World!");
  }
}
```

##### **Step 5、编译 HelloWord**

> **javac HelloWord.java**

##### **Step 6、bytecode 编译为可执行二进制文件**

> **native-image HelloWorld**

```
Build on Server(pid: 1442, port: 37055)*
[helloworld:1442]    classlist:   1,864.74 ms
[helloworld:1442]        (cap):   1,266.82 ms
[helloworld:1442]        setup:   2,971.37 ms
[helloworld:1442]   (typeflow):   6,571.88 ms
[helloworld:1442]    (objects):   4,586.49 ms
[helloworld:1442]   (features):     330.47 ms
[helloworld:1442]     analysis:  11,762.72 ms
[helloworld:1442]     (clinit):     232.68 ms
[helloworld:1442]     universe:     654.50 ms
[helloworld:1442]      (parse):   1,134.79 ms
[helloworld:1442]     (inline):   2,198.70 ms
[helloworld:1442]    (compile):  10,638.70 ms
[helloworld:1442]      compile:  14,814.72 ms
[helloworld:1442]        image:   1,844.21 ms
[helloworld:1442]        write:     294.19 ms
[helloworld:1442]      [total]:  34,514.15 ms
```

##### **Step 7、运行二进制文件**

> ./helloworld

> _遇到 **./helloworld** 执行不了可使用 **chmod +x helloworld** 修改执行权限_

```
Hello, World!
```

##### **Step 8、测试可执行二进制文件和字节码文件的运行时间对比**

> time java HelloWorld

```
Hello, World!

real	0m0.123s
user	0m0.111s
sys	0m0.028s
```

> time ./helloworld

```
Hello, World!

real	0m0.002s
user	0m0.000s
sys	0m0.002s
```