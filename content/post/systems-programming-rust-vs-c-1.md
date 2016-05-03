+++
date = "2016-05-02T22:10:46-05:00"
title = "Learning Systems Programming: Rust vs C (Part 1)"
tags = [ "rust" ]
+++

## Learning Systems Programming: Rust vs C (Part 1)

I've been wanting to learn systems programming for a long time now. After reading about Rust every-other-day on Hacker News I've been dying to dive into it but have found that most tutorials online get kind of tough to follow fot those of us without a C/C++ background. So, I found a copy of [The C Programming Language](http://www.amazon.com/Programming-Language-Brian-W-Kernighan/dp/0131103628) on Amazon for 4 bucks and will be learning both C and Rust at the same time by doing the excercises in Kernighan & Ritchie's highly praised 2nd edititon book in C and Rust side by side.

All of the code can be found on https://github.com/oldsjam/learnsystems

### Chapter One

#####1-1
Not much to say here, I do like rust's println! macro which saves you from an ugly "\n" in your code.

1-1.c
```c
#include <stdio.h>

main() {
  printf("hello, world\n");
}
```

1-1.rs
```rust
fn main() {
  println!("hello, world");
}
```