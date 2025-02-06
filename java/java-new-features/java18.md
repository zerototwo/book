---
description: Java 18 于 2022 年 3 月 22 日发布，作为短期支持版本（非 LTS），带来了 编码一致性、开发者工具改进、性能优化 等正式特性
cover: >-
  https://images.unsplash.com/photo-1727206407683-490abfe0d682?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3Mzg4MzQ2Mzl8&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Java18

## 📌 Java 18 正式特性总结表格

| **类别**   | **新特性**                  | **作用**                                              |
| -------- | ------------------------ | --------------------------------------------------- |
| **编码规范** | 默认字符集 UTF-8（JEP 400）     | 统一所有 Java 平台的默认字符集为 UTF-8，避免跨平台编码问题，提高一致性。          |
| **开发工具** | 内置简易 Web 服务器（JEP 408）    | 提供 `jwebserver`，可快速启动静态 Web 服务器，便于开发测试。             |
| **文档优化** | Java API 文档代码片段（JEP 413） | Javadoc 新增 `@snippet`，提升代码示例的可读性和维护性。               |
| **反射优化** | 方法句柄优化反射机制（JEP 416）      | 使用 `MethodHandle` 重新实现 `java.lang.reflect`，提高性能。    |
| **安全性**  | 弃用终结器（JEP 421）           | 计划移除 `finalize()`，鼓励使用 `try-with-resources` 进行资源管理。 |

## 1.JEP 400：默认字符集 UTF-8

### 作用

* 统一所有 Java 平台的默认字符集为 UTF-8，避免因不同操作系统默认编码不同而导致的兼容性问题。

### 代码示例

```java
// Java 18 之前，默认字符集依赖于操作系统
System.out.println(Charset.defaultCharset()); // Windows 可能是 GBK，Linux/macOS 可能是 UTF-8

// Java 18 之后，默认字符集固定为 UTF-8
```

⚠️ 影响

* 以前在 Windows 上运行 Java 代码时，默认编码可能是 GBK，但在 Java 18 之后，全部变成 UTF-8，可能会影响读取旧文件的代码。

## 2.JEP 408：内置简易 Web 服务器

### 作用

* 提供一个内置的静态 Web 服务器，适用于前端开发、API 调试等快速测试场景。

### 代码示例

```sh
# 启动默认 Web 服务器（监听 8000 端口）
jwebserver

# 指定服务器根目录和端口
jwebserver -d /home/user/public_html -p 8080
```

⚠️ 限制

* 仅支持静态文件，不支持 CGI、Servlet 或其他动态 Web 技术。

## 3.JEP 413：Java API 文档代码片段

### 作用

* 提升 Javadoc 代码示例的可读性，支持 @snippet 标签。

### 代码示例

```java
/**
 * 示例方法展示了如何使用 {@code Optional.isPresent}：
 * {@snippet :
 * if (v.isPresent()) {
 *     System.out.println("Value: " + v.get());
 * }
 * }
 */
public void exampleMethod() {
    // 方法实现
}
```

⚠️ 适用场景

* 提升 Java API 文档质量，方便开发者阅读示例代码。

## 4.JEP 416：方法句柄优化反射机制

### 作用

* 提升 java.lang.reflect 反射 API 的性能，改为基于 MethodHandle 实现。

### 代码示例

```java
Method method = MyClass.class.getMethod("hello");
MethodHandle handle = MethodHandles.lookup().unreflect(method);
handle.invokeExact(new MyClass()); // 更高效的反射调用
```

⚠️ 影响

* 减少反射调用的开销，对框架（如 Spring、Hibernate）性能优化有帮助。



## 5.JEP 421：弃用终结器

### 作用

* 计划移除 finalize() 方法，鼓励使用 try-with-resources 来进行资源管理。

### 代码示例

```java
// Java 18 之前（不推荐）
@Override
protected void finalize() throws Throwable {
    System.out.println("Finalizer is called!");
}

// Java 18 之后（推荐使用 try-with-resources）
try (BufferedReader reader = new BufferedReader(new FileReader("test.txt"))) {
    System.out.println(reader.readLine());
} // 资源自动关闭
```







