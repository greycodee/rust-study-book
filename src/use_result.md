## 使用 Result 来处理异常
在 Rust 中，可以使用 `Result` 这个 `enum` 来处理异常

源码如下:
```rust
pub enum Result<T, E> {
    /// Contains the success value
    #[lang = "Ok"]
    #[stable(feature = "rust1", since = "1.0.0")]
    Ok(#[stable(feature = "rust1", since = "1.0.0")] T),

    /// Contains the error value
    #[lang = "Err"]
    #[stable(feature = "rust1", since = "1.0.0")]
    Err(#[stable(feature = "rust1", since = "1.0.0")] E),
}
```

它是一个泛型，有两个类型参数，第一个是成功的类型 `T`，第二个是失败的类型 `E`

## 使用案例
我们可以这样使用它，例如我定义了一个函数，返回 `Result`
```rust
fn test(flag: bool) -> Result<String,io::Error> {
    if flag {
        let s = String::from("greycode");
        return Ok(s);
    }else{
        return Err(io::Error::new(io::ErrorKind::Other, "customize error"));
    }
}
```
当函数参数传入 `true` 时，就返回 `OK`，否则就返回 `Err`

### 使用 `match` 来处理
可以使用 `match` 匹配 `Result` 的两种情况
```rust
fn main(){
    let s = match test(false) {
        Ok(s) => s,
        Err(e) => panic!("error: {}", e),
    };
    println!("{}", s);
}
```
### 使用 `unwrap` 来处理
可以使用 `unwrap` 来处理 `Result`，如果是 `Ok`，就返回 `Ok` 的值，如果是 `Err`，`unwrap` 会自动帮我们调用 `panic` 宏
```rust
fn main(){
    let s = test(false).unwrap();
    println!("{}", s);
}
```

## 完整代码
```rust
use std::io;

fn main(){
    let s = test(true).unwrap();
    println!("{}", s);

    // let s = match test(true) {
    //     Ok(s) => s,
    //     Err(e) => panic!("error: {}", e),
    // };
    // println!("{}", s);
}

fn test(flag: bool) -> Result<String,io::Error> {
    if flag {
        let s = String::from("greycode");
        return Ok(s);
    }else{
        return Err(io::Error::new(io::ErrorKind::Other, "customize error"));
    }
}
```

## 符号 `?` 的作用
`?` 和 `unwrap()` 类似，可以简化 `match` 的写法，它的作用是，如果是 `Ok`，就返回 `Ok` 的值，如果是 `Err`，就返回 `Err` 的值，**不会调用 `panic` 宏**

> 只能用于返回值为 `Result` 的函数

**简化前:**
```rust
use std::io;
use std::io::Read;
use std::fs::File;

fn read_username_from_file() -> Result<String, io::Error> {
    let f = File::open("hello.txt");

    let mut f = match f {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut s = String::new();

    match f.read_to_string(&mut s) {
        Ok(_) => Ok(s),
        Err(e) => Err(e),
    }
}
```

**简化后:**
```rust
use std::io;
use std::io::Read;
use std::fs::File;

fn read_username_from_file() -> Result<String, io::Error> {
    let mut f = File::open("hello.txt")?;
    let mut s = String::new();
    f.read_to_string(&mut s)?;
    Ok(s)
}
```