---
title: Rust Advanced
date: 2024-10-24 15:41:13
tags: 
---

## 智能指针 Smart Pointers

首先，指针是一个包含了内存地址的变量，该内存地址引用或者指向了另外的数据。

智能指针本质是一种数据结构，但它不仅包含一个指针，还附带一些额外的元数据和功能。不同于普通指针，智能指针在 Rust 中实现了 Deref（解引用）和 Drop（清理）两个 trait（特性），这使得他们可以像指针一样解引用并在离开作用域是自动清理资源。

### 为什么存在智能指针？

- 资源管理：Rust 是静态编译语言，在编译期间就需要确定变量数据的大小以及何时释放，智能指针在普通指针的基础上，还包含了当前长度（len）、最大长度（capacity）等信息这些都是用来确定指针类型指向的那块数据所需的内存 size 的字段。以及智能指针还实现了解引用（Deref）和清理（Drop）的特性，这就让智能指针拥有了自动管理内存的能力。
- 

dyn??

[Visualizing memory layout of Rust's data types [See description/first comment]](https://www.youtube.com/watch?v=rDoqT-a6UFg&t=118s)
