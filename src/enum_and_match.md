# enum 和 match

## 定义一个 enum 枚举

### 常规枚举
```rust
enum IpAddrKind {
    V4,
    V6,
}
```

### 带数据的枚举
- 枚举的每个成员可以有不同的数据类型
```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}


enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

```
- 也可以带srtuct结构体
```rust
struct Info {
    addr: String,
    desc: String,
}
enum IpAddr {
    V4(Info),
    V6(Info),
}
```

## 使用枚举

### 使用枚举值
```rust
enum IpAddr {
    V4,
    V6,
}
    
fn main() {
    let v4 = IpAddr::V4;
}
```
打印枚举和打印结构体一样，要在枚举上加上 `#[dervice(Debug)]`
```rust
#[derive(Debug)]
enum IpAddr {
    V4,
    V6,
}
    
fn main() {
    let v4 = IpAddr::V4;
    let v6 = IpAddr::V6;
    println!("{:?}", v4);
    println!("{:?}", v6);
}
```

## Option枚举
这是定义在 [rust 标准库](https://doc.rust-lang.org/std/option/enum.Option.html)中的枚举，用于处理可能不存在的值

**源码：**
```rust
pub enum Option<T> {
    None,
    Some(T),
}
```
**使用：**
```rust
fn main() {
    let user = Option::Some("i am a user");
    match user {
        Some(user) => println!("{}", user),
        None => println!("none"),
    }
}
```

## match
