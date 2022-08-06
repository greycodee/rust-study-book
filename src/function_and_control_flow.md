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