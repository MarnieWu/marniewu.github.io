---
title: Rust 基础 Rust Basic
date: 2024-10-23
tags:
    - basic
---

## 入门 Getting Started

### Rust

静态类型语言（statically typed）：在编译时就确定了所有表达式的类型

强类型语言（strongly typed）：一切皆需要明确的类型声明

- rustc：rust 编译器（compiler）
  - 检查 rust 是否安装成功：`rustup --version`
  - 将某个 rust 文件编译成二进制（binary）文件：`rustc <file_name>.rs`

- rustup：rust 安装器和版本管理工具
  - `rustup update` 升级 rust
  - `rustup doc` 打开本地 rust 文档

- 命名规则（naming rule）：蛇形式（snake_case）
- 语句（statement）：一定要加分号（semicolon），表示执行某个操作但没有返回值的某个指令
  - 表达式语句：以分号结尾的表达式
  - 声明语句
- 表达式（expression）：主要用于计算求值（expression-base language）

### Crate

用 rust 写的库或者可执行程序 a rust library or executable program

crate.io - rust 官方包注册表

### Cargo

rust 包管理器 rust package manager

- 支持管理项目依赖 `cargo update`
- 支持编译、运行项目 `cargo build, cargo run`
- 支持上传 crates.io `cargo publish`
- Cargo.toml：dependencies declaration file

```rust
// setup a new project dir
cargo new <project_name>

// compile project [debug or release version]
cargo build [--release]

// compile project and execute [debug or release version]
cargo run [--release]

// update all dependencies (keep semi-version 0.8.5 -> 0.8.latest)
cargo update
```

## 通用概念 Common Concepts

### 变量 Variables

默认都是不可变变量（immutable variables），不用声明类型

```rust
let x = 5;
println!("The value of x is: {}", x);
```

声明可变变量（mutable variables），在运行时才会确定变量的值

```rust
let mut x = 5;
println!("The value of x is: {}", x);

x = 6; // 若这句不运行，则 x 值还是 5
println!("The value of x is: {}", x);
```

### 作用域规则 Scoping Rules

作用域是一个变量在程序中的有效范围，从变量声明的点开始知道当前作用域的结束就是一个有效范围。

作用域范围一般分为 函数作用域、代码块作用域。

不可变变量、可变变量、常量都是遵循这个作用域规则

```rust
fn foo() {
  let foo_global_variable :i32 = 1;
  
  {
    // the start of the block
    let foo_local_variable :i32 = 2; // defined in current block, only can access in current block
    println!("The value of foo_global_variable is: {}", foo_global_variable); // ✅ access anywhere within foo_block
    println!("The value of foo_local_variable is: {}", foo_local_variable);
  } // the end of the block
  
  println!("The value of foo_local_variable is: {}", foo_local_variable); // ❌ outer of the block
}
```

### 遮蔽 Shadowing

let 不可变变量支持重定义（遮蔽 shadowing）：虽然是不可变变量，但是可以用过去的值重新赋值，且可更改类型（与 `mut` 的不同之处）

```rust
let spaces = "   ";
let spaces = spaces.len();
```

块内遮蔽（shadowing）也遵循作用域规则，块内重定义变量的操作结果对外是屏蔽的，在块外访问到的还是原值

```rust
fn foo() {
    let x: i32 = 1;

    {
        // the start of the block
       // 遮蔽（shadowing）
        let x = x + 1;
        println!("The value of x is: {}", x); // ✅ The value of x is: 2
    } // the end of the block

    println!("The value of x is: {}", x); // ✅ The value of x is: 1
}
```

### 常量 Constants

声明常量必须要带类型（data types）注释（annotation），在编译时期就确定了变量的值，命名规范大写蛇形式（UPPER_SNAKE_CASE）

编译后是直接替换值

```rust
// u32: unsigned 32-bit integer 无符号32位整数
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;

println!("{}", THREE_HOURS_IN_SECONDS); // 🎯 编译后：println!("{}", 60 * 60 * 3)
```

### 静态变量 Static

特点如上同常量

作用域（scope）：在整个程序中共享，可以再多个线程之间共享 —— 即 全局变量！！

编译后是指向引用地址

```rust
static A : i32 = 1; // 假设这个 static 变量分配的内存地址为 0x123

println!("{}", A); // 🎯 编译后：存在对 A 的引用，指向 A 的内存地址
```

### 数据类型 Data Types

#### 原始类型 Primitive Types

##### 整数型 Integer Types

默认 `i32`

如果可能是负数则用 `i[bit]`，反之用 `u[bit]`，位数由运行代码的电脑决定

```rust
// TODO: 二进制位数和[整/负]数大小范围的关系
```

##### 浮点数型 Floating-Point Types

默认 `f64`

```rust
let x = 2.0; // f64
let y: f32 = 3.0; // f32
```

##### 布尔型 The Boolean Types

```rust
// both are 👌🏻
let a = true;
let b: bool = true;
```

##### 文本类型 Textual Types

`char` 字符型

`unicode` 编码，1 char = 4 bytes，默认单引号包裹的文字（单个文字）表示字符型变量，也可以显式声明类型,

```rust
let c = 'z';
let z: char = 'ℤ';
let heart_eyed_cat = '😻';
```

#### 复合类型 Compound Types

🕵🏻‍♂️ ：下面字符串类型建议看完所有权部分知识，再去理解比较好！！

##### Character encoding format

- ASCII：1 character = 7 bits binary，total 128 characters（max decimal number of 7-bit binary number）
- UTF-8：1 character = 1～4 bytes = (1～4) x 8 bits，total 1,112,064 character

#### 🎯 TODO：Memory management & ownership in Rust

![image-20241010155641561](/images/image-20241010155641561.png)

<https://www.linkedin.com/pulse/memory-management-rust-amit-nadiger#:~:text=The%20.,string%20literals%20and%20other%20constants>.

【TODO】compile binary --> read-only memory 是什么？

【TODO】Machine words 是什么？

【TODO】Rc smart pointer  reference counted pointer

[Visualizing memory layout of Rust's data types](https://www.youtube.com/watch?v=rDoqT-a6UFg&t=118s)

##### `String` 文本字符串型

一串动态长度的 UTF-8 编码序列，用来表示文本字符串。UTF-8 编码规则用 1～4 个字节表示一个字符，是默认拥有所有权的类型。

动态分配大小所以存储在 heap 上，对应栈上的变量存储指针（ptr = 内存地址）、长度（len = 实际长度）、容量（capacity = 可用长度）。

``` rust
// String 类型值内置了修改和其他的各种方法
// mutable variable
let mut s: String = String::from("hello");
// modify string s
s.push('!');
// replace first character of string s
s.replace_range(0..1, "H");
// check the result
println!("s = {}", s);  // ✅ 输出：s = Hello!
```

##### `String Slice` 文本字符串切片类型

我们把某个字符串的某段字符串称为 slice（字符串切片/字符串片段）。上面提到的，文本字符串本身是动态长度的 UTF-8 编码序列，且拥有数据的所有权。当我们只需要借用字符串文本数据本身时，我们需要使用 `&str` 类型，它包含指针（ptr = 该 slice 第一个字符在内存中对应的地址）、长度（len = 从第一个字符开始，一共多少个字符），slice 因为是借用关系，本身是不可变的。字符串本身是动态大小的类型，有了 slice 的借用类型之后，编译时就可以计算出对应字符串所占字节大小，这对 Rust 这种强静态编译语言是必需的。它有两种具体使用场景：

1. 普通文本字符串（string literal）：不可修改
2. 作为某个 `String` 类型的切片字符串：不可修改

可以看到，都是不可修改，所以当我们有要修改字符串的计划，我们就应该使用上面 `String` 类型，而不修改的话就选择 `&str` 类型。

```rust
fn str_and_string() {
    // 普通文本字符串 string literals
    let normal_str = "hello, ";
    // 可操作的、拥有所有权的 String 类型字符串
    let mut mutable_string = String::from(normal_str);

    // a string slice of String, 没有修改字符串的计划
    let hello = &mutable_string[0..5];
    println!("hello = {}", hello);

    // 修改字符串 &mut 才能修改
    do_some_mutation(&mut mutable_string);
    // 只需要借用字符串的值
    only_use_text(&mutable_string);
    println!("mutable_string = {}", mutable_string);
}

fn do_some_mutation(input: &mut String) {
    input.replace_range(0..1, "H");
    input.push_str("add this to the end");
}

fn only_use_text(input: &String) {
    let hello = &input[..=6];
    let world = "world";
    println!("greet = {}", [hello, world].concat());
}

```

隐式类型转换 `String` -> `&str`

当函数参数定义为 `&str` 切片类型时，我们在传入的是完整切片时是可以直接传入 `String` 类型字符串的，相当于 `&s[..]`，Rust 实现了自动解引用，在必要时会将 `&String` 自动转换成 `&str` 。这样使得，如果接受参数是字符串的引用，我们采用 `&str` 作为入参，但能获得两种类型数据的兼容。

``` rust
let s :String = String::from("hello, world");
// String 转换成 slice
let slice = s.as_str();
```

注意：反过来是不行的，因为 `String` 类型 转 切片类型，没有转换成本，而从 切片类型 转 `String` 类型还需要为其重新申请内存，对性能是有损耗的，Rust 不会自动做这个行为。

##### 与其他类型的转换

![image-20241013075143718](/images/image-20241013075143718.png)

##### 元组 The Tuple Type

混合类型 —— 多种类型的数值的集合，一旦定义了不能改变长度

```rust
// 声明
let tup: (i32, f64, u8) = (500, 6.4, 1);

// 取值(1)
let (x, y, z) = tup;
println!("The value of y is: {y}"); // ✅ The value of y is: 6.4
// 取值(2)
println!("Values in tup: {} {} {}", tup.0, tup.1, tup.2); // ✅ Values in tup: 500 6.4 1
```

##### 数组 The Array Type

数组的定义其实就是为分配一段连续的相同数据类型的内存块。

`[T; N]` 表示 `N` 个值的数组，每个值的类型为 `T`。数组长度一经定义不可修改，所以编译时数组占用的内存大小就已是确定的了，不能追加或删除元素，但可以修改元素的值。

```rust
// 先声明类型和长度，后赋值
let a: [i32; 5] = [1, 2, 3, 4, 5]; // 定义后无法再修改大小

// 同样的值，可以简化创建
let a = [3; 5]; // 5 个 3

// 取值
let first = a[0];
let second = a[1];


// 转迭代器，得到 (index, value) 枚举对 list
a.iter().enumerate()
```

遍历方式

```rust
// 已知长度
let arr = [10; 4];

for i in 0..4 {
  println!("{}", arr[i]);
}

// 未知长度：迭代器
for i in arr.iter() {
 println!("{}", i);
}
```

数组的引用都是值的传递，即和原数组没有联系，原数组不受任何影响

```rust
fn test_arr() {
    let mut a = [10, 20];
    let mut b = a;

    a[0] = 0;
    println!("a {:?}", a); // [0, 20]
    println!("b {:?}", b); // [10, 20]

    let arr = [10, 20, 30];
    println!("1111 {:?}", arr); // [10, 20, 30]

   // 参数传递，是直接复制 arr 的值传入，和 arr 无关
    update(arr);
    println!("2222 {:?}", arr); // [10, 20, 30]

    fn update(mut arr: [i32; 3]) {
        for i in 0..3 {
            arr[i] = 0;
        }
        println!("update {:?}", arr); // [0, 0, 0]
    }
}
```

##### 向量 The Vec Type

An array-like data structure，一种类数组数据结构，和数组的区别是它支持动态大小，即可以增加或删除元素。

```rust
fn main() {
    /*
      使用Vec和HashMap实现书籍库管理系统
      添加书籍，查询库存，更新库存，删除书籍
    */
  
    let books = vec![
        "The Rust Programming Language",
        "Welcome to Rust",
        "Rust by Example",
    ];

    struct BookSystem {
        map: HashMap<String, i32>,
    }

    impl BookSystem {
        // 创建
        fn new(books: Vec<&str>) -> BookSystem {
            let mut map = HashMap::new();

            for book in books {
                map.insert(book.to_string(), 1);
            }

            BookSystem { map }
        }
        // 添加书籍
        fn add(&mut self, name: &str) {
            self.map.insert(name.to_string(), 1);
        }
        // 查询库存
        fn find(&self, name: &str) -> i32 {
            match self.map.get(&name.to_string()) {
                Some(num) => *num,
                None => 0,
            }
        }
        // 更新库存
        fn update(&mut self, name: &str, num: i32) {
            self.map.insert(name.to_string(), num);
        }
        // 删除书籍
        fn delete(&mut self, name: &str) {
            self.map.remove_entry(name);
        }
        fn log(&self) {
            println!(">>>>>> Current stock <<<<<<");
            for (k, v) in &self.map {
                println!("[{}]: {}", k, v);
            }
        }
    }

    let mut sys = BookSystem::new(books);
    sys.log();
    let book_name = "Easy Rust";
    sys.add(book_name);
    sys.log();
    sys.update(book_name, 3);
    sys.log();
    println!("The stock of [{}] is {}", book_name, sys.find(book_name));
    sys.delete(book_name);
    sys.log();
}
```

##### 切片 The Slice Type

表示 `String` 类型、`Array` 类型、`Vec` 类型 的局部数据，`&T` 只引用值，`&mut T` 可以修改原数据

```markdown
切片常用函数
- len(): 取 slice 元素个数
- is_empty(): 判断 slice 是否为空
- contains(): 判断是否包含某个元素
- repeat(): 重复 slice 指定次数
- reverse(): 反转 slice
- join(): 将各元素压平（flatten）并通过指定的分隔符连接起来
- swap(): 交换两个索引处的元素，如 s.swap(1, 3)
- windows(): 以指定大小的窗口进行滚动迭代
- start_with(): 判断 slice 是否以某个 slice 开头
```

##### 自定义类型 Custom Types

Rust 自定义数据类型主要通过 `struct` 和 `enum` 关键字形成

- `struct`: 定义一个结构体型数据
- `enum`: 定义一个枚举型数据

###### 结构体 Structures

基础用法，结构体可以定义 n 多个字段

- 结构体变量和一般变量一样，默认也是不可变的（immutable），`let mut T = struct XXX` 定义可变的（mutable）结构体变量
- 可以没有任何字段名，但不能没有字段类型，这种结构体长的像元组，所以被被称为元祖结构体（tuple struct），例如：`struct Point(i32, i32, i32)` 表示一个点的坐标 `x,y,z`
- 可以不包含任何字段，即没有类型也没有名称，被称为单元结构体

``` rust
struct Person {
    name: String,
    age: u8,
}
let me = Person { name: "Marnie".to_string(), age: 10 };

// A unit struct
struct Unit;

// A tuple struct
struct Pair(i32, f32);
let a = Pair(8, 0.2);
println("{}", a.0);

// A struct with two fields
struct Point {
    x: f32,
    y: f32,
}
let p = Point { x: 0.5, y: 1.2 };

// Structs can be reused as fields of another struct
struct Rectangle {
    // A rectangle can be specified by where the top left and bottom right
    // corners are in space.
    top_left: Point,
    bottom_right: Point,
}
let point_top_left = Point { x: 0.5, y: 1.2 };
let point_bottom_right = Point { x: 1.8, y: 0.2 };
let rect = Rectangle { top_left: point_top_left, bottom_right: point_bottom_right };
```

结构体更新语法：我们可以用 `..structA` 给 `structB` 进行更新，在 `structB` 中没有显示声明的字段，都会从 `structA` 中取 。

但是注意了📢，在 Rust 中严格管控着数据所有权（ownership），所以当针对没有 copy 特性的字段被用来赋值之后，该字段的数据所有权即会发生转移。只要有一个字段发生了所有权变化，那么整个结构体就已经是不完整的了，该结构体本身就不能再被转移，即无法整个结构体直接使用。

没有 copy 特性（trait）指的就是那些存在 heap 上的数据，在 stack-side 存储的是其内存块的 pointer，赋值操作即原指针失效，内存地址转移到新指针上。

``` rust
   let user1 = User {
        active: true,
        username: String::from("example1"),
        email: String::from("example1@examples.com"),
        sign_in_count: 1,
    };
    let user2 = User {
        email: String::from("example2@examples.com"),
        ..user1
    };
```

结构体可以包含行为，即带有方法（方法和函数是有区别的）的结构体

###### 枚举型 Enum

结构体通常是描述带有不同字段的某个类型，而枚举型指的是某个更上层的类型下会包含多个不同类型，比如动物类型包含猫猫、狗狗、兔子、猪等等

```rust
enum Animal {
  Dog,
  Cat,
  Rabbit,
  Pig,
}
```

### 函数 Functions

#### 语句 Statements

表示一个要执行的操作，没有返回值

```rust
fn foo() {
  // ❌ 首先 let y = 1; 是个语句，没有返回值，且（）中加表达式只能在 if、while 条件判断下使用
  let x = (let y = 1);
  
  // 等同于 let x = 1; let y = 1;
  let x = y = 1;
}
```

### 流程控制 Control Flow

#### 条件控制 `if`

`if` 表达式的条件（condition）必须是 `bool` 类型

```rust
let number = 3;

// ❌ error[E0308]: mismatched types, expected `bool`, found integer"
if number {
  println!("Never print: expected `bool`, found integer");
}
```

`if or else` 表达式后紧跟的代码块被称为 arms，和 `match` 表达式的 arms 一致

`if` 表达式的代码块最后一行若是表达式，即为当前块的返回值

若是多个 `if...else if` 每个代码块的返回值类型需要一致

```rust
fn if_else_expressions() -> () {
    let number = 55;
    const NOT_MATCHED_MESSAGE: &str = "Not Matched";

    let message = if number % 4 == 0 {
        "Divide by 4"
    } else if number % 3 == 0 {
        "Divide by 3"
    } else if number % 2 == 0 {
        "Divide by 2"
    } else {
        NOT_MATCHED_MESSAGE
    };

    if message != NOT_MATCHED_MESSAGE {
        println!("{}", message);
    }
}
```

 `todo()!` 表示延迟实现当前代码块，和占位符概念类似，即可以跳过类型检查

```rust
fn foo() {
  todo()!;
}
```

#### 循环 Loop

#### `loop`

`loop` 加大括号声明循环（代码块可以有返回值），`continue` 跳出本次循环，`break` 跳出并终止整个循环，

```rust
fn loop_expressions() -> () {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 10; // 终止循环，并有返回值
        }
    };

    println!("{}", result);
}
```

嵌套循环，用 `单引号 + label` 的格式命名，`break LOOP_NAME` 跳出对应循环

```rust
fn nested_loops() -> () {
    let mut count = 0;

    'counting_up: loop {
        println!("count = {}", count);
        let mut remaining = 10;

        loop {
            println!("remaining = {}", remaining);

            if remaining == 9 {
                break; // 结束并跳出当前循环
            }

            if count == 2 {
                break 'counting_up; // 结束并跳出 'counting_up 循环
            }

            remaining -= 1;
        }

        count += 1;
    }
}
```

#### `while`

如果需要判断某些条件的循环，可以使用 `while` 更方便

```rust
fn while_case() -> () {
    let mut count = 1;

    while count < 12 {
        count += 1;
    }

    println!("count = {}", count);
}
```

#### `for`

`for loop` 遍历数组

```rust
// loop in array
fn for_loop() -> () {
    let a = [10, 20, 30, 40, 50];

    for e in a {
        println!("i={}", e);
    }

   // 1..=5 快速创建一个1-5的
    for n in 1..=5 {
        println!("n={}", n);
    }
  
   // (1..=5).rev() 快速创建一个5-1倒序的数组
    for n in (1..=5).rev() {
        println!("n={}", n);
    }
}

// loop in vector
fn vector_case() -> () {
    let mut v = vec![1, 2, 3];

    // 表示并非所有权转移，& 表示借用，借用是只读
    for vv in &mut v {
        // vv 是对 v 中的某个 vector 的引用，
        // 引用的本质是一个指针，指针不能直接和值进行比较，
        // 需要加 *号 解引用，解引用就表示该引用变量的值
        if *vv == 1 {
            *vv = 5;
        }
        println!("v={}", vv);
    }

    println!("{}", v[0]);
}
```

### 所有权 Ownership

所有权是控制一个 Rust 程序如何管理内存的一套规则。所有程序都要有管理它们在运行时使用计算机内存的方式。

#### 内存管理方式 Memory Management

常见的内存管理方式：

- 垃圾回收（Garbage Collection）：如 JavaScript、Java 这种动态语言，语言层面设计了一套内存回收机制，垃圾回收器会持续追踪内存的分配并定期找到不再被使用的内存进行释放回收，让开发者无需去关心内存的分配和回收
- 手动管理（Manual Management）：如 C、C++ 这种静态语言，需要手动调用分配和释放内存的方法进行操作，这让开发者需要对内存的分配及释放负责，忘记释放会导致内存浪费，太早释放又会导致获得无效的变量

第三种 —— Rust 使用的内存管理方式：所有权系统（Ownership System）

- 所有者（ower）指的是这块内存（memory）是属于谁的，包括访问和修改的能力
- 每个值都有一个所有者，任何时候都只能有一个有效所有者
- 当所有者离开作用域（scope），值会被丢弃（drop）
- 遵循所有权系统规则，编译器在编译时通过静态分析管理内存安全，违背规则的程序不会通过编译
- 通过借用（borrowing，即引用）机制来共享数据

#### 🎯 TODO：内存分配方式 Memory Allocation

- 栈（Stack）：连续的内存空间，从低地址到高地址逐步分配，简单数据类型会被直接存储在栈上，便于操作
- 堆（Heap）：非连续内存空间，申请多少大小空间就一次性分配多少，会通过一个指针指向这一块内存空间，复杂数据类型的值会在堆上开辟对应的空间存放，而栈上对应的这个变量存的是与这块内存相关的指针信息，即内存地址（adress）、大小（Capacity）、长度（Length）
- Static：blahblahblah...

#### 转移 或 复制 Move or Copy

基于不同的内存分配方式，针对数据和变量之间的赋值操作，在 Rust 中也会有两种表现情况：

- 简单数据类型：一般是固定大小的数据，值存放在栈上，赋值是对值的拷贝（copy）
- 复杂数据类型：一般为非固定大小数据，值被存放在堆上，该内存一旦分配，赋值的操作默认被看做为转移（move），即将该内存的所有权转移给某个变量

```rust
// copy
let a: i32 = 1;
let b = a; // directly copy 1

// move
let a: String = String::from("Hello");
let b = a; // move the ownership of "Hello" from a to b
// current, a is considered as transferred, which means it's no longer needed
```

其实可以简单理解，转移（move）是所有权（ownership）系统实现 “内存所有者只能有一个” 的一种规则机制，这个机制防止出现数据竞争的可能 —— 多个线程对统一资源进行访问和修改，从而导致程序结果与期望不符。本身具有所有权的变量本质是存在栈上的某个指针类型变量，它的作用是为了指向堆中的某块内存，所以一经转移，相当于指针不再指向这块内存，这个变量就不再有效了，即不可以再被访问。

#### 所有权和生命周期（Lifetimes）

所有权是指某块内存的所有者，所有者是某个变量，而变量的生命周期和所在作用域相关，在某个作用域下定义的变量只能在该作用域下使用。所有权也有着相应的限制，变量在某个作用域（scope）内被分配到对应数据的内存，在离开作用域后，变量会变得无效，它会交还对某个数据内存的所有权，则该“无主”的数据内存会被视为不再需要的，会触发自动释放内存的机制，也就是回收内存。

```rust
fn foo() {
  let s = String::from("Hello"); // s is valid from this point
  // s is the owner of the string "Hello"
} // this scope is over now, s and string "Hello" are not valid
// the data is not valid means it is no longer needed, which will trigger the step of freeing memory
```

当变量被传入某个函数，也是发生了转移操作，意味着数据的所有权被传递给了函数，函数外已经没有权利访问和修改该变量所指的数据。如下例子：

![image-20241005203754332](/images/image-20241005203754332.png)

第一步，`s` 拥有了 `string "Hello"` 的所有权

第二步，将 `string "Hello"` 的所有权转移给了 `carry_params` 函数

而 `carry_params` 没有对参数 `s` 做任何处理，那么在函数执行完毕后，离开作用域，入参 `s` 对应也不再有效，对应占用的数据内存会被释放

第三步，我们再打印 `s` 就会得到值已被转移了，即无权访问的编译错误提示

#### 拥有所有权的作用

所有权是指对某块内存的所有，即拥有读写这块内存的权力，当我们需要修改某个数据时，我们就需要拥有其所有权，才能有修改的能力。而当我们只需要读，而不需要写，那就不必拥有或者转移其所有权。可以看到，有了所有权规则的加持，代码变得更加安全但也因此增加了书写代码的难度。

上面提到如果我们不需要获得某个数据的所有权，只是想要访问值，可以用另一个概念 —— 借用（borrow）。

#### 借用 Borrow

当我们不需要数据的所有权时，我们使用借用来访问数据的值，借用分为两种：

- borrow：不可变的借用，我只是把数据借给你用 `&T`
- borrow mutably：可变的借用，我不仅把数据借给你用，且给你修改它的权限 `&mut T`，这种借用也可以被叫做引用（reference）

其实可以理解为，`&` 给了读数据的权力，`&mut`  在读的基础上给了写的权力，但数据（也就是这块内存）的拥有者并没有变。

```rust
fn str_and_string() {
    // 普通文本字符串 string literals
    let normal_str = "hello, ";
    // 可操作的、拥有所有权的 String 类型字符串
    let mut mutable_string = String::from(normal_str);

   // a string slice of String,
    // 借用（引用） "hello, "，没有修改字符串的需求
    let slice = &mutable_string[..];
    println!("slice = {}", slice);
    // 借用对象 和 被借用对象 都是指向同一块内存，所以相等
    assert_eq!(slice, mutable_string);

    // 1️⃣ 默认借用，只需要借用字符串的值，不涉及所有权
    only_use_text(&mutable_string);

   // 2️⃣ 可变的借用，不仅可以访问值，还给了修改值的权力
   // 需要声明 &mut 才能修改字符串
    do_some_mutation(&mut mutable_string);
    println!("mutable_string = {}", mutable_string);
}

fn do_some_mutation(input: &mut String) {
    input.replace_range(0..1, "H");
    input.push_str("add this to the end");
}

fn only_use_text(input: &String) {
    let hello = &input[..=6];
    let world = "world";
    println!("greet = {}", [hello, world].concat());
}

```

### 🎯 TODO：生命周期 Lifetimes

##### NLL —— Non-Lexical Lifetimes

##### Dangling Referencing 悬垂引用

指的是一个变量是有效状态的时间，即可以被使用的时长，通常也被叫做存活（live）时间。

生命周期是 Rust 用来保证引用有效性的机制。生命周期注解允许编译器推断引用的有效范围，确保在引用仍然有效的时候使用它们。

在 Rust 中，一个变量的存活时间与其作用域（scope）和是否还存在被使用（引用 references）有关。

- 作用域规则决定了一个变量能否被访问，在某个作用域下定义的变量也只在这个作用域内有效，即离开作用域后无法被访问，会自动被销毁。

- 当编译器检查到一个变量不再存在任何被引用被借用关系后，该变量也会被自动销毁。

```rust
let x :i32 = 5;
// 存在被使用
println!("x = {}", x); // after this line, x has no more references, will be automatically destroyed

let y :i32 = 6;
```

#### 借用规则 Borrow Rules

- 一个变量只能存在一个可变引用或者多个不可变引用，不能同时存在可变和不可变引用

#### 借用检查器 Borrow Checker

前面说了，借用是用于不关心所有权的场景下的，那没有所有权就意味着我需要依赖被我借用的那个变量，当它不合法时、无效时，那我的借用行为也是不可能通过的。Borrow Checker 便是在编译时，检查借用是否合法，即被借用的变量是否还在有效的生命周期内。结合前面提到的所有权的生命周期，来看下截图上的例子：

![image-20241010163617345](/images/image-20241010163617345.png)

可以看到报错部分说的是被借用值 x 存活的不够长，重点看下黄色框内，模拟画的作用域结构图。

- `r` 在 a 作用域中创建，即它存活在作用域 a 存活的期间，即 foo 函数执行期间
- `x` 在 b 作用域中创建，即它存活在作用域 b 存活的期间，作用域 b 是个 block 作用域，在它执行完成后内部变量都会自动释放，即 `x` 在作用域 b 外无效

因为 `r` 是个借用类型，Borrow Checker 会检查被借用变量是否是有效的，当发现被借用变量的生命周期小于借用变量的生命周期，即 `x` 的生命周期小于 `r` 的生命周期，这是不合法的 —— 报错。所以如果是存在借用关系的两个变量，那么被借用变量的存活时间一定要比借用变量存活的时间更长，存活时间更长可以归纳为两种情况：

- 相同作用域范围下，被借用变量定义早于借用变量
- 借用变量的作用域层级深于被借用变量（越深销毁越早）

##### 📸 TODO 手动管理可能会遇到的问题

内存泄露

悬空指针

缓冲区溢出

## 泛型（Generics）和特质（Traits）

### Match

### Option

```rust
// `unwrap()`: 有值的时候会取值，没值的时候会 `panic`
let mut s = String::from("hello");
let p1 = s.pop().unwrap();
println!("{}", p1);
```

## 返回值与错误处理

### 可恢复的错误 `Result<T, E>`

从系统全局角度来看是可以接受的错误，这些错误只会影响某个用户自身的操作进程，一般是逻辑层面上的错误（业务逻辑、操作逻辑），不会对系统的全局稳定性产生影响。

#### 使用 Result 返回值

### 不可恢复的错误 `panic!`

全局性或者系统性的错误，列入数组越界访问、影响启动流程的错误等等，这些错误是影响系统流程的。

#### 主动触发 panic

Rust 提供了 `panic!` 宏，当调用执行该宏时，程序会打印出一个错误信息，展开报错点前面的函数调用堆栈，最后退出程序。

```rust
fn main() {
  panic!("crash and burn");
}
```

#### backtrace 栈展开

默认程序执行调用栈输出是开启 `--debug` 的时候才会展示，可以通过 `RUST_BACKTRACE=1 cargo run` 来手动开启调用栈回溯。

栈展开也称栈回溯，包含了函数调用的顺序，按照逆序排列：最近调用的函数在列表最上方。

#### `unwrap` 方法

- `Option` : 有值的时候会取值，没值的时候会调用 `panic!`
- `Result` : `Ok` 会返回 `Ok` 中的值，`Err` 会为我们调用 `panic!`

```rust
   let arr = [1, 2, 3, 4];
   let a = first(&arr).unwrap();
   println!("The first element is {}", a);
```

#### `expect` 方法

用于设置预期的 panic 错误文案

```rust
    let file = File::open("hello.txt").expect("Failed to open hello.txt");
    println!("{:?}", file);
```

#### 传播错误

`unwrap` 方法在遇到错误情况下都会直接调用 `panic!` 从而会直接终止程序的进行，但很多情况下我们遇到的都是预期内的错误情况，并不需要终止，只需要将错误向上传播，这时我们就不用使用 `unwrap` 方法。取而代之，通常使用 `?` 链式调用传播错误，它的功能是问号前的表达式执行结果成功则继续向后执行，若结果失败或者说错误，则截停执行逻辑到当前表达式为止并将错误返回，此时如果前一个表达式设置了错误 callback 等类似的处理，就能触发相应的逻辑。

##### Option 和 Result 的互转

Result 有两个结果 `Ok` 和 `Err`，Option 包含 `Some` 和 `None`

- Result 转 Option，转换即将 `ok` 和 `err` 都转为 `Some`

  - `err(): Err -> Some`

  - `ok(): Ok -> Some`

```rust
    let f = File::open("hello.txt").err();
    if f.is_some() {
        println!("no file");
    }
 
    let f = File::open("hello.txt").ok();
    if f.is_none() {
        println!("no file");
    }
```

- Option 转 Result

  - `ok_or()`
