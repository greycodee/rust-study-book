## 变量的声明
先来看看下面的代码：
```rust
let hello = "早上好";
hello = "晚上好";
println!("{}",hello);
```
以我多年的编程经验，看了又看，好像没什么问题，但是运行时却报如下错误
```rust,noplayground
   Compiling playground v0.0.1 (/playground)
error[E0384]: cannot assign twice to immutable variable `hello`
 --> src/main.rs:4:1
  |
3 | let hello = "早上好";
  |     -----
  |     |
  |     first assignment to `hello`
  |     help: consider making this binding mutable: `mut hello`
4 | hello = "晚上好";
  | ^^^^^^^^^^^^^^^^ cannot assign twice to immutable variable

For more information about this error, try `rustc --explain E0384`.
error: could not compile `playground` due to previous error
```
编译器告诉我们，这个变量不能定义两次，如果要多次定义的话，需要使用 `mut` 关键字，于是我们按照编译器的要求，改成如下代码：
```rust
let mut hello = "早上好";
hello = "晚上好";
println!("{}",hello);
```
编译通过并成功输出**晚上好**。

> **自动推断变量类型**: 可以看到我们在声明变量时，使用了 `let` 关键字，这个关键字告诉编译器，这个变量是一个可变的（mutable）变量，这样编译器就可以自动推断出这个变量的类型。

## 常量
常量遵循如下规则：
- 常量的值是不可变的
- 常量可以在任何范围内声明，包括全局范围
- 常量定义时必须指定数据类型
- 常量命名约定是在单词之间使用全大写和下划线

```rust
const TEST_NUMBER:i32 = 123;
println!("{}",TEST_NUMBER);
```
## 变量遮蔽
Rust 允许声明相同的变量名，在后面声明的变量会遮蔽掉前面声明的，如下所示：
```rust
let x = 1;
let x = x+1;
println!("{}",x);
```

## 基础数据类型
Rust 有四种主要的基础类型：整数、浮点数、布尔值和字符。
### 整数
直接用一张表格来说明 Rust 的整数类型：

| 长度|有符号类型	|无符号类型|
|----|----|----|
|8-bit	|i8	|u8|
|16-bit	|i16|	u16|
|32-bit	|i32|	u32|
|64-bit	|i64|	u64|
|128-bit|	i128|	u128|
|arch	|isize	|usize|

其中 `i` 和 `u` 开头分别代表有符号和无符号类型的数据。有符号类型就是说明这个整数包括负数，而无符号类型就是说明这个整数不包括负数。
其中有 `isize` 和 `usize` 这两个类型，它可以根据你当前运行代码的系统来自动定义整数的类型长度，如果您使用的是 64 位架构，则数据长度为 64-bit；如果您使用的是 32 位，则数据长度为 32-bit。

我们可以和使用其他类型来定义数据一样来使用它：
```rust
let x:isize = 5;
println!("{}",x);
```
如果你不知道怎么选择数据的长度，那么我们一般可以不指定整数的类型，这时它的默认类型为 `i32`
```rust
let x = 5;
println!("{}",x);
```

#### 更多表示方式
当然除了上面那些常规的表示方式，Rust 还支持如下表示方式：

|数据 |例子|
|----|----|
|十进制	|98_222|
|十六进制	|0xff|
|八进制	|0o77|
|二进制|	0b1111_0000|
|Byte (u8 only)|	b'A'|

如果十进制数据太长的话可以用 `_` 符号来分割，比如上面的例子中的 `98_222`，其实还是 `98222`，我们还可以换成我们比较能一眼读出的 `9_8222`
```rust
let a = 98_222;
let b = 9_8222;
println!("a: {}\nb: {}",a,b);
```
如果要直接是用其他进制的值，只需在对应进制数据前加上一个常用的标识符即可，例如：
- 十六进制：`0x`
- 八进制：  `0o`
- 二进制：  `0b`
```rust
let a = 0xFF;
let b = 0o77;
let c = 0b1111_0000;

println!("a: {}\nb: {}\nc: {}",a,b,c);
```
当然如果你想直接输出进制数据，我们可以直接这样写:
> 输出大小十六进制用：`:#X`,输出小写十六进制：`:#x`

```rust
let a = 0xFF;
let b = 0o77;
let c = 0b1111_0000;

println!("a: {:#X}\nb: {:#o}\nc: {:#b}",a,b,c);
```
当我们想输出一个字节的数据时，我们可以用 `b` 标识符来表示，例如：
```rust
let a = b'A';
println!("a: {}",a);

// 输出十六进制值
println!("a HEX: {:#X}",a);
```
### 浮点数
Rust 支持两种浮点数类型：`f32` 和 `f64`。

f32 代表 32 位浮点数，f64 代表 64 位浮点数。

```rust
let x = 20.0;
let y:f32 = 21.0;
println!("x: {:.1}\ny: {:.1}",x,y);
``` 
浮点数根据 IEEE-754 标准实现。f32 类型是单精度浮点型，f64 为双精度。
### 布尔值
Rust 当然也和其他语言一样支持布尔类型啦。
用法：
```rust
let x = true;
// or
let y:bool = false;
println!("x: {}\ny: {}",x,y);
```

### 字符

```rust

let a = 'a';
let b: char = 'q'; // with explicit type annotation
let c = '😻';

println!("a: {}\nb: {}\nc: {}",a,b,c);
```
请注意，我们 char 使用**单引号**指定文字，而不是使用双引号的字符串文字。Rust 的char类型大小为 4 个字节，表示一个 Unicode 标量值，这意味着它可以表示的不仅仅是 ASCII。在 Rust 中，重音字母、中文、日文、韩文字符、表情符号和零宽度空格都是 `char` 的有效值。Unicode 标量值的范围从 U+0000 到 U+D7FF 和 U+E000 到 U+10FFFF 在内。
这个 `char` 只能表示单个字符，如果用它来表示多个字符的话就会发生错误
```rust
let hello = '你好';
println!("hello: {}",hello);
```
```
   Compiling playground v0.0.1 (/playground)
error: character literal may only contain one codepoint
 --> src/main.rs:3:13
  |
3 | let hello = '你好';
  |             ^^^^^^
  |
help: if you meant to write a `str` literal, use double quotes
  |
3 | let hello = "你好";
  |             ~~~~~~

error: could not compile `playground` due to previous error

```
编译器提示这是字符串，可以用双双引号来表示，例如：
```rust
let hello = "你好";
println!("hello: {}",hello);
```

## 复合数据类型
除了基础数据类型，Rust 还支持两种复合数据类型，如：元组（Tuple）和数组（Array）。
### 元组
元组是将具有多种类型的多个值组合成一个复合类型的一般方法。元组具有固定长度：一旦声明，它们的大小就不能增长或缩小。

元组里面可以包含不同类型的数据，然后用括号 `()` 包围，比如：
```rust
let tup = (1,2.0,true,"hello");
println!("tup: {:?}",tup);
println!("tup[0]: {}",tup.0);
println!("tup[1]: {:.1}",tup.1);
println!("tup[2]: {}",tup.2);
println!("tup[3]: {}",tup.3);
```
当然我们在定义元组时也可以指定元组中每个位置数据的类型，例如：
```rust
let tup:(i32,f32,bool,&str)=(1,2.0,true,"hello");
println!("tup: {:?}",tup);
```
我们也可以对元组进行**结构**，将元组中的值分别放入不同的变量中，例如：
```rust
let tup = (1,2.0,true,"hello");
let (x,y,z,w) = tup;
println!("x: {}\ny: {:.1}\nz: {}\nw: {}",x,y,z,w);
```

### 数组
与元组不同，数组的每个元素都必须具有**相同的类型**。与其他一些语言中的数组不同，Rust 中的数组具有**固定长度**，也就是说，一旦声明，它们的大小就不能增长或缩小。（如果需要增大或缩小可以使用 Rust 中的向量类型）

在数组中，用方括号 `[]` 来包围数据，用逗号 `,` 来分割每个数据，比如：
```rust
let arr = [1,2,3,4,5];
println!("arr: {:?}",arr);
```
我们也可以指定数组的类型和大小：
```rust
let arr:[i32;5] = [1,2,3,4,5];
println!("arr: {:?}",arr);
```
此时就必须严格按照数组的大小和类型来声明数组，比如下面这些声明都是错误的：
```rust,noplayground
// ❌ 预定义的数组大小不匹配
let arr:[i32;5] = [1,2,3,4];

// ❌ 预定义的数组类型不匹配
let arr:[i32;5] = [1.0,2.0,3.0,4.0,5.0];
```
也可以初始化一拥有相同默认值和指定长度的数组：
```rust
// 初始化长度为 5 ，默认值为 0 的数组
let arr = [0;5];
println!("arr: {:?}",arr);
```