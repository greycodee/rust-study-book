# Struct 结构体

## 定义一个结构体
使用 `struct` 关键字定义一个结构体
```rust
struct User{
    name: String,
    age:    u8,
}
```

## 使用 struct
```rust
struct User{
    name: String,
    age:    u8,
}

fn main(){
    let user = User{
        name: String::from("greycode"),
        age:20
    };
    println!("name:{},age:{}",user.name,user.age);
}
```
## 可变结构体
使用 `mut` 关键字定义一个可变结构体
```rust
struct User{
    name: String,
    age:    u8,
}

fn main(){
    let mut user = User{
        name: String::from("greycode"),
        age:20
    };
    println!("name:{},age:{}",user.name,user.age);
    user.age = 30;
    println!("name:{},age:{}",user.name,user.age);
}
```
## 结构体初始化简写
当变量名和结构体中的 field 字段名称一致时，可以使用简写
```rust
struct User{
    name: String,
    age:    u8,
}

fn main(){
    let name = String::from("greycode");
    let age = 22;
    let user = build_user(name, age);
    println!("name:{},age:{}",user.name,user.age);
}

fn build_user(name:String,age:u8) -> User{
    User {
        name, // 使用简写赋值
        age, // 使用简写赋值
    }
}
```
## 使用结构 Update 语法从其他实例创建实例
当我们要创建两个相同类型的结构体实例但是有小部分字段值不同的实例时，通常我们这样创建：
```rust
struct User{
    name: String,
    age:    u8,
    sex:    String,
    email: String,
}

fn main(){
    let user1 = User{
        name:String::from("greycode"),
        age:20,
        sex:String::from("man"),
        email:String::from("ssss@gmail.com"),
    };
    let user2 = User{
        name:String::from("jack"),
        age:user1.age, // 因为 age 字段是 u8 类型,实现了 Copy trait，所以这里是复制操作，user1.age 还可以访问
        sex:user1.sex, // 这里是移动操作，所以 user1.sex 所有权移交到这里，user.sex 失效
        email:user1.email, // 这里是移动操作，所以 user1.email 所有权移交到这里，user.email 失效
    };
    // 到这里时，user1 实例里只有 user1.name 和 user1.age 有效
}
```
会出现很多从 user1 实例中取值的代码
这时候我们可以直接使用下面这种写法：
```rust
struct User{
    name: String,
    age:    u8,
    sex:    String,
    email: String,
}

fn main(){
    let user1 = User{
        name:String::from("greycode"),
        age:20,
        sex:String::from("man"),
        email:String::from("ssss@gmail.com"),
    };
    let user2 = User{
        name: String::from("greycode"),
        ..user1 // 移交 user1.name 字段以外所有 user1 字段的所有权
    };
}
```

## 元组结构体
```rust
struct Color(i32,i32,i32);
struct Point(i32,i32,i32);

fn main(){
    let black = Color(0,0,0);
    let origin = Point(0,0,0);
}
```

## 类单元结构体
```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}

```
## 打印结构体
在开发时，可以在结构体上加上 `#[derive(Debug)]` ，然后使用 `:?` 来打印一个结构体
```rust
#[derive(Debug)]
struct User{
    name: String,
    age:    u8,
}

fn main(){
    let user = User{
        name: String::from("greycode"),
        age:20
    };
    println!("{:?}",user)
}
```

## 使用impl定义方法
可以使用 `impl` 关键字来定义结构体的方法，同时可以同时定义多个 `impl` 块
```rust
struct User {
    name: String,
    age: u32,
}

impl User {
    fn new(name: String, age: u32) -> User {
        User { name, age }
    }
}
impl User {
    fn say_hello(&self) {
        println!("Hello, my name is {} and I am {} years old.", self.name, self.age);
    }
}
    
fn main() {
    // 当方法返回的是结构体本身时，要用 :: 来调用
    let user = User::new(String::from("John"), 30);
    println!("{} is {} years old", user.name, user.age);

    user.say_hello();
}
```

