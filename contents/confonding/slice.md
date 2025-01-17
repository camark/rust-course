# 切片和切片引用
关于 `str` / `&str`，`[u8]` / `&[u8]` 区别，你能清晰的说出来嘛？如果答案是 No ，那就跟随我一起来看看切片和切片引用到底有何区别吧。

> 在继续之前，参见[此处](https://course.rs/basic/compound-type/string-slice.html#切片slice)了解何为切片

切片允许我们引用集合中部分连续的元素序列，而不是引用整个集合。例如，字符串切片就是一个子字符串，数组切片就是一个子数组。

## 无法被直接使用的切片类型
Rust 语言特性内置的 `str` 和  `[u8]` 类型都是切片，前者是字符串切片，后者是数组切片，下面我们来尝试下使用 `str` ：
```rust
let string: str = "banana";
```

上面代码创建一个 `str` 类型的字符串，看起来很正常，但是编译就会报错：
```shell
error[E0277]: the size for values of type `str` cannot be known at compilation time
 --> src/main.rs:4:9
  |
4 |     let string: str = "banana";
  |         ^^^^^^ doesn't have a size known at compile-time
```

编译器准确的告诉了我们原因：`str` 字符串切片它是 [`DST` 动态大小类型](https://course.rs/advance/custom-type.html#动态大小类型)，这意味着编译器无法在编译期知道 `str` 类型的大小，只有到了运行期才能动态获知，这对于强类型、强安全的 Rust 语言来说是不可接受的。

也就是说，我们无法直接使用 `str`，而对于 `[u8]` 也是类似的，大家可以自己动手试试。

总之，我们可以总结出一个结论：**在 Rust 中，所有的切片都是动态类型，它们都无法直接被使用**。

那么问题来了，该如何使用切片呢？

## 切片引用
何以解忧，唯有引用。由于引用类型的大小在编译期是已知的，因此在 Rust 中，如果要使用切片，就必须要使用它的引用。

`str` 切片的引用类型是 `&str`，而 `[i32]` 的引用类型是 `&[i32]`，相信聪明的读者已经看出来了，`&str` 和 `&[i32]` 都是我们非常常用的类型，例如:
```rust
let s1: &str = "banana";
let s2: &str = &String::from("banana");

let arr = [1, 2, 3, 4, 5];

let s3: &[i32] = &arr[1..3];
```

这段代码就可以正常通过，原因在于这些切片引用的大小在编译器都是已知的。

## 总结
我们常常说使用切片，实际上我们在用的是切片的引用，我们也在频繁说使用字符串，实际上我们在使用的也是字符串切片的引用。

总之，切片在 Rust 中是动态类型 DST，是无法被我们直接使用的，而我们在使用的都是且切片的引用。

| 切片 | 切片引用|
| --- | ---   |
| str 字符串切片 | &str 字符串切片的引用 | 
| [u8]  数组切片| &[u8] 数组切片的引用 | 


**但是出于方便，我们往往不会说使用切片引用，而是直接说使用字符串切片或数组切片，实际上，这时指代的都是切片的引用！**