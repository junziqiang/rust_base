### 泛型
```rust
// 基础用法与C++类似
fn largest<T: PartialOrd + Copy> (list: &[T]) -> T {  
    let mut largest = list[0];  
    for &item in list.iter() {  
        if item > largest {  
            largest = item;  
        }  
    }  
    largest  
}  
  
fn main() {  
    let number_list = vec![34, 50, 25, 100, 65];  
    let result = largest(&number_list);  
    println!("The largest number is {}", result);  
    let char_list = vec!['y', 'm', 'a', 'q'];  
    let result = largest(&char_list);  
    println!("The largest char is {}", result);  
}
```
```rust
// 结构体中使用泛型
struct Point<T, U> {  
    x: T,  
    y: U,  
}  
  
impl<T, U> Point<T, U> {  
    fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {  
        Point {  
            x: self.x,  
            y: other.y,  
        }  
    }  
}  
  
fn main() {  
    let p1 = Point { x: 5, y: 10.4 };  
    let p2 = Point { x: "Hello", y: 'c'};  
  
    let p3 = p1.mixup(p2);  
  
    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);  
}
```
### trait
> trait类似于接口

```rust
// trait定义
// 也可以有默认的实现
pub trait Summary { fn summarize(&self) -> String; }
 
pub struct NewsArticle {  
    pub headline: String,  
    pub location: String,  
    pub author: String,  
    pub content: String,  
}  

// 为类型实现trait
impl Summary for NewsArticle {  
    fn summarize(&self) -> String {  
        format!("{}, by {} ({})", self.headline, self.author, self.location)  
    }  
}  
  
pub struct Tweet {  
    pub username: String,  
    pub content: String,  
    pub reply: bool,  
    pub retweet: bool,  
}  
  
impl Summary for Tweet {  
    fn summarize(&self) -> String {  
        format!("{}: {}", self.username, self.content)  
    }  
}
```

1. trait做为参数（类似于传入父类）语法为
```rust
// 普通写法
pub fn notify(item: impl Summary) {
	println!("Breaking news! {}", item.summarize());
}
// trait bounds// 如果是要求实现了多个trait中间使用+号
pub fn notify<T: Summary>(item: T) {
	println!("Breaking news! {}", item.summarize());
}

pub fn notify<T: Summary + testSymmary>(item: T) {
	println!("Breaking news! {}", item.summarize());
}
pub fn notify<T>(item: T)
where T: Summary + testSymmary
{
	println!("Breaking news! {}", item.summarize());
}
// 返回实现了trait的类型
fn returns_summarizable() -> impl Summary
```
2. 对任何实现了特定 trait 的类型有条件地实现 trait
```rust
impl<T: Display> ToString for T { // --snip-- }
```
### 生命周期
> Rust 中的每一个引用都有其 **生命周期**

```rust
// 生命周期标注
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {  
    if x.len() > y.len() {  
        x  
    } else {  
        y  
    }  
}
```
**生命周期省略**
- 每一个是引用的参数都有它自己的生命周期参数
- 如果只有一个输入生命周期参数，那么它被赋予所有输出生命周期参数
- 如果方法有多个输入生命周期参数并且其中一个参数是 `&self` 或 `&mut self`，说明是个对象的方法(method) 那么所有输出生命周期参数被赋予 `self` 的生命周期