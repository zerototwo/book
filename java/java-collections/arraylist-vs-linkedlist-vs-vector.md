---
description: >-
  在 Java 中，ArrayList、LinkedList 和 Vector 都是 List
  接口的实现类，但它们的底层实现和适用场景不同。本文详细对比它们的 数据结构、性能、线程安全性、使用场景 等。
cover: >-
  https://images.unsplash.com/photo-1736160388319-c99c1dd44099?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3Mzg4NzQzNTJ8&ixlib=rb-4.0.3&q=85
coverY: 0
---

# ArrayList vs LinkedList vs Vector

### 📌 ArrayList vs LinkedList vs Vector（对比表格）

| **特性**                | **ArrayList**    | **LinkedList**     | **Vector**                    |
| --------------------- | ---------------- | ------------------ | ----------------------------- |
| **底层数据结构**            | **动态数组**（数组扩容）   | **双向链表**           | **动态数组**（线程安全）                |
| **是否线程安全**            | ❌ **非线程安全**      | ❌ **非线程安全**        | ✅ **线程安全**（使用 `synchronized`） |
| **查找（get/indexOf）**   | **快（O(1)）**      | **慢（O(n)）**        | **快（O(1)）**                   |
| **插入/删除（add/remove）** | **慢（O(n)）**      | **快（O(1) - O(n)）** | **慢（O(n)）**                   |
| **扩容机制**              | **1.5 倍扩容**      | **不需要（链表动态扩展）**    | **2 倍扩容**                     |
| **遍历方式**              | **for 循环 & 迭代器** | **适合迭代器**          | **for 循环 & 迭代器**              |
| **适合场景**              | **随机访问多，插入删除少**  | **插入删除多，随机访问少**    | **多线程环境**                     |

## 1.ArrayList（动态数组）

### 特点

* 底层是 Object\[] 数组
* 查询快（O(1)）🚀，但插入/删除慢（O(n)）
* 非线程安全
* 支持动态扩容（1.5 倍）
* 适用于 随机访问频繁 的场景

### 代码示例

```java
import java.util.ArrayList;

public class ArrayListExample {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("A"); // O(1)
        list.add("B");
        list.add("C");

        list.remove(1); // O(n)（删除索引 1 处元素，后续元素前移）

        System.out.println(list); // [A, C]
    }
}
```

📌 适用场景

* 适合查询多，插入/删除少的场景
* 适用于大多数日常开发需求（如存储订单、用户列表等）

## 2.LinkedList（双向链表）

### 特点

* 底层是双向链表（Node 节点）
* 插入/删除快（O(1)）🚀，但查询慢（O(n)）
* 非线程安全
* 占用更多内存（每个节点存储 data + next + prev）
* 适用于 频繁插入/删除 的场景

### 代码示例

```java
import java.util.LinkedList;

public class LinkedListExample {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<>();
        list.add("A");
        list.addFirst("B"); // O(1)（头部插入）
        list.addLast("C");  // O(1)（尾部插入）

        list.remove("B");   // O(1)（删除头部元素）

        System.out.println(list); // [A, C]
    }
}
```

📌 适用场景

* 适合插入/删除多，查询少的场景（如消息队列）
* 适用于需要 双向遍历 或 LIFO 结构（如栈、队列）

## 3.Vector（线程安全动态数组）

### 特点

* 底层是 Object\[] 数组
* 线程安全（所有方法都使用 synchronized）
* 查询快（O(1)），但插入/删除慢（O(n)）
* 扩容机制：2 倍扩容
* 适用于 多线程环境

### 代码示例

```java
import java.util.Vector;

public class VectorExample {
    public static void main(String[] args) {
        Vector<String> list = new Vector<>();
        list.add("A");
        list.add("B");

        list.remove(1); // O(n)（删除索引 1 处元素）

        System.out.println(list); // [A]
    }
}
```

📌 适用场景

* 适合多线程环境
* 如果没有多线程要求，推荐 ArrayList（更快）

## 5.总结



| **使用场景**           | **推荐选择**                            |
| ------------------ | ----------------------------------- |
| **随机访问频繁**（如数据库缓存） | ✅ `ArrayList`                       |
| **插入/删除频繁**（如消息队列） | ✅ `LinkedList`                      |
| **多线程环境**（如共享数据结构） | ✅ `Vector` 或 `CopyOnWriteArrayList` |
| **大数据量存储**         | ✅ `ArrayList`                       |
| **需要顺序存储并且遍历多**    | ✅ `ArrayList`                       |
