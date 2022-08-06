## 函数

### 函数定义
在 Rust 中，用 `fn` 关键字来定义函数。
```rust
fn my_function() {
    println!("Hello, world!");
}
fn main() {
    my_function();
}
```
下面是一个带参数的函数，在函数入参区，先定义参数名，然后在定义参数的类型：
```rust
fn my_function(name: &str){
    println!("hello,{}",name);
}
fn main(){
    my_function("world");
}
```
### 函数返回值
函数可以将值返回给调用它们的代码。我们不命名返回值，但我们必须在箭头 `->` 之后声明它们的类型。在 Rust 中，函数的返回值与函数体块中的最终表达式的值同义。可以通过使用 `return` 关键字并指定一个值来提前从函数返回，但大多数函数会隐式返回最后一个表达式,如下面的例子：
```rust
fn my_function() -> i32{
    9
}
fn main(){
    let x=my_function();
    println!("{x}");
}
```
可以看到，我们的 `my_function()` 函数并没有使用 `return` 关键字来返回指定的值，这样的话 Rust 会隐式返回最后一个表达式的值。注意这边最后一个表达式的值末尾不能带 `;` 符号。
### 函数返回多个值
如果我们想返回多个值的时候，我们可以用元组来返回，然后在调用的时候进行解构：
```rust
fn my_function() -> (i32,f32){
    (9,3.14)
}
fn main(){
    let (x,y)=my_function();
    println!("x: {}\ny: {}",x,y);
}
```


## `if` 语句
`if` 语句中的判断条件**不需要**用括号包围，但是代码块需要用大括号包围。
```rust
let x = 3;

if x == 7 {
    println!("x is 7");
} else {
    println!("x is not 7");
}
```
也可以使用 `else if` 语句来添加判断条件
```rust
let x = 3;

if x == 7 {
    println!("x is 7");
} else if x == 8 {
    println!("x is 8");
}else {
    println!("x is not 7 or 8");
}
```
在让我们看看下面的例子：
```rust
// ❌ 错误写法
let condition = true;
let mut x = condition?3:5;
println!("x is {}",x);
```
如果熟悉其他语言的朋友就知道这是一个标准的**三元表达式**，是不是感觉这样写没什么问题？但是当我们执行代码时却会发生错误。

在 Rust 中，可以用 if 语句来实现类似的表达式
```rust
let condition = true;
let y = if condition { 3 } else { 5 };
println!("y is {}",y);
```  

## 循环
Rust 中有三种循环：loop、while 和 for。
这三种循环各有各的侧重点：
- **loop**：如果需要无限循环，且不需要退出循环时（当然在 loop 中可以用 `break` 和 `continue` 来退出当前循环或整个循环 ），可以使用 loop 循环。像比如端口监听，我们可以使用 loop 循环来监听端口，直到收到一个新的连接。loop 循环还可以拥有返回值，可以将它赋值给一个变量。
- **while**：如果需要循环，但是当满足某个条件要退出时，可以使用 while 循环。
- **for**：如果要遍历一个数组时，可以用 for 循环。
### `loop` 语句
最基础的用法：
```rust,noplayground
loop {
    println!("more and more!")
}
```
上面代码会不断的输出 *more and more!*，如果要停止输出，只能终止这个程序运行。

#### 跳出 `loop` 循环

当然我们也可以在 `loop` 循环体里添加 `if` 语句，然后用 `break` 跳出循环。
```rust
let mut x = 0;
loop {
    if x >= 5{
        break;
    }
    println!("more and more!");
    x += 1;
}
```
上面代码只会输出 5 遍 *more and more!*，然后就跳出循环。

#### `loop` 循环返回值
`loop` 还有一特性是它可以在结束循环时，使用 `break` 返回一个值
```rust
let mut x = 0;
let y = loop {
    if x >= 5{
        break "kill";
    }
    println!("more and more!");
    x += 1;
};
println!("{y}");
```

#### `loop` 循环嵌套标签
当我们有多个 `loop` 循环嵌套时，我们可以为 `loop` 循环定义一个标签，然后使用 `break` 来跳出指定标签下的循环。
```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        'remain:loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```
标签的命名以符号 `'` 开头。

### `while` 语句
在上面的 `loop` 循环中，我们通过在循环体中编写 `if` 语句来判断是否跳出循环体，如果你不想编写 `if` 语句，那么可以考虑使用 `while` 循环。
```rust
let mut x = 0;
while x < 5 {
    println!("hello");
    x+=1;
}
```
`while` 循环也支持嵌套写：
```rust
let mut x = 0;
while x < 2 {
    println!("hello");
    x += 1;
    let mut y = 0;
    while y < 3{
        println!("world!");
        y += 1;
    }
}
```

`while` 循环也可以使用循环标签，通过 `break` 跳出指定循环：
```rust
let mut x = 0;
'is_x:while x < 2 {
    println!("hello");
    x += 1;
    let mut y = 0;
    'is_y: while y < 3{
        println!("world!");
        if y == 1{
            break 'is_x;
        }
        y += 1;
    }
}
```
### `for` 语句
如果要遍历一个数组或字符串时，可以用 `for` 循环。
```rust
let arr = [1,3,5,7,9];
for i in arr {
    println!("curr is {}",i);
}
```
或者这样
```rust
for i in 1..5 {
    println!("curr is {}",i);
}