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