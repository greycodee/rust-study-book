## 使用案例
```rust

// 定义 trait
trait Hello{
    fn say_hi(&self);
}

// 定义实现 trait 的结构体
struct Bob{
    name: String,
}

impl Hello for Bob{
    fn say_hi(&self){
        println!("Hi, I'm {}", self.name);
    }
}

struct Jack{
    name: String,
}

impl Hello for Jack{
    fn say_hi(&self){
        println!("Hi, I'm {}", self.name);
    }
}

// 这样写是错误的
// struct Person<T: Hello>{
//     pub all_person: Vec<T>,
// }
// impl <T> Person<T> where T: Hello{
//     pub fn run(&self){
//         for person in &self.all_person{
//             person.say_hi();
//         }
//     }
// }
struct Person{
    pub all_person: Vec<Box<dyn Hello>>,
}
impl Person{
    pub fn run(&self){
        for person in &self.all_person{
            person.say_hi();
        }
    }
}

fn main() {
    let all = Person{
        all_person: vec![
            Box::new(Bob{name: "Bob".to_string()}),
            Box::new(Jack{name: "Jack".to_string()}),
        ],
    };
    all.run();
}


```