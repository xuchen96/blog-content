---
title: Java 基础知识
date: 2026-05-21
summary: Java 核心基础知识梳理，涵盖面向对象、集合框架、异常处理和多线程等关键概念。
tags: [Java, 基础, 面向对象, 集合, 多线程]
author_email: 1393270387@qq.com
slug: java-basics
published: true
---
## Java 语言概述

Java 是一门**面向对象**的编程语言，具有**跨平台**（Write Once, Run Anywhere）的特性。源代码（`.java`）编译为字节码（`.class`），在 JVM 上运行。

### Java 三大体系

| 体系 | 全称 | 用途 |
|------|------|------|
| Java SE | Standard Edition | 桌面应用、基础核心 |
| Java EE | Enterprise Edition | 企业级 Web 应用 |
| Java ME | Micro Edition | 嵌入式/移动设备 |

---

## 面向对象三大特性

### 1. 封装（Encapsulation）

隐藏对象的内部状态，通过 `private` 字段 + `public` getter/setter 对外暴露有限的访问接口。

```java
public class User {
    private String name;
    private int age;

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) {
        if (age < 0) throw new IllegalArgumentException("年龄不能为负数");
        this.age = age;
    }
}
```

### 2. 继承（Inheritance）

子类通过 `extends` 复用父类的属性和方法，支持单继承（一个类只能有一个直接父类）。

```java
public class Animal {
    public void eat() { System.out.println("吃东西"); }
}

public class Dog extends Animal {
    @Override
    public void eat() { System.out.println("狗吃骨头"); }
}
```

### 3. 多态（Polymorphism）

同一个行为具有不同的表现形式，通过**方法重写**（Override）和**方法重载**（Overload）实现。

```java
// 多态：父类引用指向子类对象
Animal a = new Dog();
a.eat();  // 输出：狗吃骨头

// 重载：同方法名，不同参数
public int add(int a, int b) { return a + b; }
public double add(double a, double b) { return a + b; }
```

---

## 抽象类与接口

| 特性 | 抽象类（abstract class） | 接口（interface） |
|------|--------------------------|-------------------|
| 实例化 | 不能 | 不能 |
| 构造方法 | 有 | 没有 |
| 方法实现 | 可以有具体方法 | Java 8+ 支持 default 方法 |
| 多继承 | 不支持 | 支持实现多个接口 |
| 成员变量 | 任意类型 | 默认 `public static final` |

```java
// 接口定义规范
public interface Flyable {
    void fly();
    default void land() { System.out.println("默认着陆方式"); }
}

// 抽象类提供公共实现
public abstract class Bird {
    public abstract void sing();
    public void breathe() { System.out.println("呼吸"); }
}
```

---

## 集合框架

### 常用集合类

```java
// List —— 有序，可重复
List<String> list = new ArrayList<>();
list.add("A"); list.add("B"); list.add("A");

// Set —— 无序，不可重复
Set<String> set = new HashSet<>();
set.add("A"); set.add("B"); set.add("A");  // size = 2

// Map —— 键值对
Map<String, Integer> map = new HashMap<>();
map.put("Java", 1); map.put("Python", 2);
```

### 各实现类对比

| 类型 | 实现类 | 底层结构 | 线程安全 | 特点 |
|------|--------|----------|----------|------|
| List | `ArrayList` | 数组 | 否 | 查询快，增删慢 |
| List | `LinkedList` | 双向链表 | 否 | 增删快，查询慢 |
| Set | `HashSet` | HashMap | 否 | 无序，O(1) 查找 |
| Set | `TreeSet` | 红黑树 | 否 | 有序（自然排序） |
| Map | `HashMap` | 数组+链表+红黑树 | 否 | 允许 null 键/值 |
| Map | `ConcurrentHashMap` | 分段/Node 锁 | **是** | 高并发首选 |

---

## 异常处理

### 异常体系

```
Throwable
├── Error          （不可处理的严重问题，如 OutOfMemoryError）
└── Exception
    ├── RuntimeException（运行时异常，如 NullPointerException）
    └── Checked Exception（编译时异常，如 IOException）
```

### try-catch-finally

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("除数不能为零：" + e.getMessage());
} finally {
    System.out.println("无论是否异常，finally 块都会执行");
}
```

> **try-with-resources**（Java 7+）：实现了 `AutoCloseable` 的资源无需手动关闭。
>
> ```java
> try (FileReader fr = new FileReader("test.txt");
>      BufferedReader br = new BufferedReader(fr)) {
>     String line = br.readLine();
> } catch (IOException e) {
>     e.printStackTrace();
> }
> ```

---

## 多线程

### 创建线程的三种方式

```java
// 1. 继承 Thread
class MyThread extends Thread {
    public void run() { System.out.println("线程运行"); }
}

// 2. 实现 Runnable
class MyRunnable implements Runnable {
    public void run() { System.out.println("线程运行"); }
}

// 3. 实现 Callable（有返回值）
class MyCallable implements Callable<String> {
    public String call() { return "执行结果"; }
}
```

### 线程池

```java
ExecutorService pool = Executors.newFixedThreadPool(5);
pool.execute(() -> System.out.println("执行任务"));
pool.submit(() -> "返回值").get();
pool.shutdown();
```

**线程池核心参数**（`ThreadPoolExecutor`）：

| 参数 | 说明 |
|------|------|
| corePoolSize | 核心线程数 |
| maximumPoolSize | 最大线程数 |
| keepAliveTime | 空闲线程存活时间 |
| workQueue | 任务等待队列 |
| RejectedExecutionHandler | 拒绝策略 |

### synchronized 与 Lock

```java
// synchronized —— JVM 内置锁
public synchronized void method() { /* 同步代码 */ }

// Lock —— 更灵活的锁控制
Lock lock = new ReentrantLock();
lock.lock();
try { /* 同步代码 */ } finally { lock.unlock(); }
```

---

## 常用关键字速查

| 关键字 | 作用 |
|--------|------|
| `static` | 修饰方法/变量/代码块，属于类而非实例 |
| `final` | 修饰类（不可继承）、方法（不可重写）、变量（不可修改） |
| `this` | 当前对象的引用 |
| `super` | 父类对象的引用 |
| `transient` | 修饰的字段不参与序列化 |
| `volatile` | 保证变量的内存可见性，禁止指令重排 |

---

## JDK 8 重要特性

- **Lambda 表达式**：`(参数) -> { 方法体 }`
- **Stream API**：链式操作集合，支持 filter/map/reduce
- **Optional**：优雅处理 `null`，避免 `NullPointerException`
- **新的日期时间 API**：`LocalDate`、`LocalTime`、`LocalDateTime`

```java
List<String> names = Arrays.asList("Java", "Python", "Go");
names.stream()
     .filter(s -> s.startsWith("J"))
     .map(String::toUpperCase)
     .forEach(System.out::println);  // 输出：JAVA
```

---

*持续更新中，欢迎收藏。*
