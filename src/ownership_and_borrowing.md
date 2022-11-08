# 所有权
rust 用来管理内存的一套机制，称为所有权（ownership）。
其他语言，例如 Java 管理内存的方式是垃圾回收（garbage collection），rust 选择了另一种方式，即所有权。
所有权的好处是，rust 编译器可以在编译时就发现内存错误，而不是在运行时才发现。
所有权的缺点是，rust 程序员需要手动管理内存，这可能会增加开发成本。

The stack stores values in the order it gets them and removes the values in the opposite order. **This is referred to as last in, first out**

## 所有权规则
- 在 Rust 中每个值都有一个所有权（owner）。
- 一个值在任意时刻有且只有一个所有权。
- 当所有权被移出一个作用域时，这个值将被丢弃。

## 变量作用域
```rust
{                   // s 在这里无效，它还没有被声明
    let s = "hello"; // s 从现在开始有效
    // do stuff with s
} // s 的作用域到里结束，s 不再有效
```


## 移动
因为 Rust 在移动一个值时，前一个变量会失效，所以这个常规的浅拷贝不一样
![trpl04-04jGV7ap](https://cdn.jsdelivr.net/gh/greycodee/images@main/2022/11/08/trpl04-04jGV7ap.svg)
## 克隆
直接调用 `clone` 方法，能够复制**堆上**的数据。
## 复制
以下是一些类型实现了 `Copy` trait:

- 所有整数类型，例如 u32.
- 布尔类型， bool, 有值 true和 false.
- 所有浮点类型，例如 f64.
- 字符类型， char.
- 元组，如果它们只包含也实现的类型 Copy. 例如， (i32, i32)工具 Copy， 但 (i32, String)才不是。

可以省略 `clone`,因为调用了也不会做任何事情
```rust
let x = 5;
let y = x; //和 let y = x.clone(); 等效
println!("x = {}, y = {}", x, y);
```

## 借用
“这种通过引用传递参数给函数的方法也被称为借用
 （borrowing）。在现实生活中，假如一个人拥有某件东西，你可以从他那里把东西借过来。但是当你使用完毕时，就必须将东西还回去。”
 比如当我们想写一个获取一个计算字符串长度的函数时，如果我们这样写的话
 ```rust
fn main() {
    let s = String::from("hello");
    let l = get_len(s); // 变量 s 这里将所有权移交给了 get_len 函数

    println!("s:{},len:{}",s,l) // 所以这里变量 s 是无效的
}

fn get_len(s:String) -> usize {
    s.len()
}
 ```

 ```
error[E0382]: borrow of moved value: `s`
 --> src/main.rs:5:28
  |
2 |     let s = String::from("hello");
  |         - move occurs because `s` has type `String`, which does not implement the `Copy` trait
3 |     let l = get_len(s);
  |                     - value moved here
4 |
5 |     println!("s:{},len:{}",s,l)
  |                            ^ value borrowed here after move
 ```