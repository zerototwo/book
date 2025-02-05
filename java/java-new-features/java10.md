---
description: >-
  Java 10 是 Java 9 之后的 短期版本，虽然没有像 Java 9
  那样的模块化变革，但它仍然带来了一些重要的语言增强、GC优化、JVM性能提升等新特性
---

# Java10

## 📌 Java 10 新特性总结表格

| **类别**        | **新特性**                                   | **作用**                |
| ------------- | ----------------------------------------- | --------------------- |
| **语法增强**      | `var` 关键字                                 | 局部变量类型推断，减少冗余类型声明     |
| **GC 改进**     | G1 GC 性能优化                                | 降低 Full GC 触发频率，提高吞吐量 |
| **JVM 改进**    | `Application Class-Data Sharing (AppCDS)` | 缩短 JVM 启动时间，提高性能      |
| **内存管理**      | 堆内存分配改进（Thread-Local Handshakes）          | 动态调整堆大小，提升并发效率        |
| **安全性**       | `Root Certificates`（默认可信 CA 证书）           | Java 默认包含 CA 证书，提高安全性 |
| **Docker 支持** | `JVM Container Awareness`                 | 使 JVM 运行时更好地适配容器环境    |

## 1.var 关键字（局部变量类型推断）

### 作用

* 允许在局部变量声明时 省略显式类型，由编译器自动推断类型
* 提高代码可读性，减少冗余代码

### 代码示例

```java
// 传统方式（Java 9 及以前）
String message = "Hello, Java 10!";
List<String> list = new ArrayList<>();

// Java 10 之后
var message = "Hello, Java 10!";
var list = new ArrayList<String>();

System.out.println(message);
```

{% hint style="danger" %}
• var 只能用于局部变量，不能用于类字段、方法参数、方法返回类型

• var 必须在声明时初始化，不能赋值 null
{% endhint %}

## 2.G1 GC 性能优化

### 作用

* 减少 Full GC 触发的频率，提高吞吐量
* 自动调整堆的回收策略，提升大对象的回收能力

```sh
java -XX:+UseG1GC MyApplication
```

### 变化点：

* 在 Java 9 中，G1 GC 可能触发 Full GC
* Java 10 优化了 G1 GC，使其更难触发 Full GC

## 3.Application Class-Data Sharing (AppCDS)

### 作用

* 缩短 JVM 启动时间
* 多个 JVM 进程共享 Class Metadata
* 提高容器化应用的启动速度

### 代码示例

```sh
java -Xshare:dump
```

运行应用时启用 AppCDS：

```sh
java -Xshare:on -cp MyApplication.jar MyMainClass
```

{% hint style="warning" %}
适用于 Microservices、云原生应用，如 Spring Boot。
{% endhint %}

## 4.堆内存分配改进（Thread-Local Handshakes）

### 作用

* 允许在不冻结所有线程的情况下执行内存回收
* 提高 高并发环境下的 GC 效率

### 启用方式

无需额外配置，Java 10 默认启用 Thread-Local Handshakes。

{% hint style="info" %}
适用于多线程应用，如 高并发 Web 服务器、分布式系统。
{% endhint %}

## 5.Root Certificates（默认 CA 证书）

### 作用

* Java 10 内置可信 CA 证书，无需手动配置
* 提高 Java HTTPS 连接的安全性

### 代码示例

无需额外代码，Java 10 默认包含可信 CA 证书。

{% hint style="info" %}
适用场景:

• HTTPS 连接

• OAuth 认证

• SSL/TLS 证书校验
{% endhint %}

## 6.Docker 适配增强

### 作用

* 使 JVM 运行时更好地适配容器（Docker）环境
* 动态调整 JVM 资源分配（CPU & 内存）

### 启用方式

（1）在 Docker 容器中运行 Java 10 应用

```sh
docker run --memory=512m --cpus=1 openjdk:10 java -XshowSettings:system -version
```

（2）JVM 识别 Docker 资源分配

```sh
java -XX:+PrintFlagsFinal | grep -i heap
```

{% hint style="info" %}
云计算、Kubernetes、Docker Swarm 运行 Java 应用
{% endhint %}







