# Guarantee capacity to a power of 2

## `tableSizeFor(int cap)` 详解

### **1️⃣方法作用**

* `tableSizeFor(int cap)` **计算大于等于 `cap` 的最小 `2` 的幂**，用于 HashMap 初始化容量计算，确保容量始终是 `2` 的幂，提高哈希表的查找性能。

### **2️⃣ 方法源码**

```java
static final int tableSizeFor(int cap) {
    int n = cap - 1;         // 1️⃣ 先将 cap 减 1，防止 cap 本身是 2 的幂
    n |= n >>> 1;           // 2️⃣ 让高位 1 扩展到低 1 位
    n |= n >>> 2;           // 3️⃣ 让高 2 位 1 扩展到低 2 位
    n |= n >>> 4;           // 4️⃣ 让高 4 位 1 扩展到低 4 位
    n |= n >>> 8;           // 5️⃣ 让高 8 位 1 扩展到低 8 位
    n |= n >>> 16;          // 6️⃣ 让高 16 位 1 扩展到低 16 位
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```

### 3️⃣ tableSizeFor(int cap) 运行流程

| **步骤**         | **运算**       | **二进制变化（以 `cap = 20` 为例）**   |
| -------------- | ------------ | ---------------------------- |
| `cap - 1`      | `19`         | `0001 0011`                  |
| \`n            | = n >>> 1\`  | `n = 0001 0011 \| 0000 1001` |
| \`n            | = n >>> 2\`  | `n = 0001 1011 \| 0000 0110` |
| \`n            | = n >>> 4\`  | `n = 0001 1111 \| 0000 0001` |
| \`n            | = n >>> 8\`  | `n = 0001 1111 \| 0000 0000` |
| \`n            | = n >>> 16\` | `n = 0001 1111 \| 0000 0000` |
| `return n + 1` | `n + 1 = 32` | `0010 0000`                  |

📌 **最终返回 `32`，是大于 `20` 的最小 `2` 的幂。**

### 4️⃣ 为什么 HashMap 需要 2 的幂？

#### 计算索引更快

```java
index = (table.length - 1) & hash;
```

若 table.length 是 2 的幂，(table.length - 1) 是全 1，使得索引计算只保留哈希值的低位，避免取模运算，提高计算效率。

### 5️⃣ 代码优化点

1.为什么要 cap - 1？

避免 cap 本身是 2 的幂，错误计算导致 tableSizeFor(16) 返回 32。

2.为什么使用 |= 和 >>>？

通过 逐步扩展最高位 1，高效计算 2 的幂，而不需要循环计算。

3.为什么使用 n + 1？

使 n 变成完整的 2 的幂，而不是 11111 这种中间状态。

## 6️⃣总结

* tableSizeFor(int cap) 使 HashMap 容量始终是 2 的幂，提高计算效率。
* |= n >>> x 操作高效计算 2 的幂，避免循环，提升性能。
* 保证索引计算 (table.length - 1) & hash 快速无冲突，提高 get/put 效率。

