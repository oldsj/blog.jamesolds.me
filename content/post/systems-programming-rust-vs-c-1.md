+++
date = "2016-05-02T22:10:46-05:00"
title = "Learning Systems Programming: Rust vs C (Part 1)"
tags = [ "rust" ]
+++

## Learning Systems Programming: Rust vs C (Part 1)

I've been wanting to learn systems programming for a long time now. After reading about Rust every-other-day on Hacker News I've been dying to dive into it but have found that most tutorials online get kind of tough to follow for those of us without a C/C++ background. So, I found a copy of [The C Programming Language](http://www.amazon.com/Programming-Language-Brian-W-Kernighan/dp/0131103628) on Amazon for 15 bucks and will be learning both C and Rust at the same time by doing the exercises in Kernighan & Ritchie's highly praised 2nd edition book in C and Rust side by side. One of the ways I learn best is to teach. Given that I am learning as I'm going, don't take everything here as the gospel. Having said that, I'll do my best to accurately describe the material. After all, that's another great benefit of teaching while you learn: you have to make sure you are confident in your understanding of a topic before teaching it as fact.

All of the code can be found on https://github.com/oldsjam/learnsystems



### 1.1 Getting Started

Right off the bat, I do like rust's println! macro which saves you from an ugly "\n" in your code. The standard library in C is opt-in while it is in scope by default in Rust. Not a huge deal but its nice to not have to explicitly include it in practically every program you write. I think it's strange to have printing done by a macro, simply because I've never seen it done that way before. Looking forward to reading more about macros and the Rust team's reasoning behind going that route vs a function/method.

example.c
```c
#include <stdio.h>

main() {
  printf("hello, world\n");
}
```

example.rs
```rust
fn main() {
  println!("hello, world");
}
```

### 1.2 Variables and Arithmetic Expressions

We get to try out some fancy features of the Rust language here with a pattern variable binding:
```rust
let (lower, upper, step) = (0.0, 300.0, 20.0)
```
and by describing the fahr variable as mutable with the mut keyword. By default, everything in Rust is immutable and if, for example you'd like a variable to be able to be changed in the future without reassigning it with let, you have to use the mut keyword. I like secure-by-default and it's not a huge inconvenience so this is great. I found the formatting this exercise a little tough in Rust. Firstly, I had to use a macro to get the float numbers to only show 1 decimal place:
```rust
format!("{:.*}", 1, ((5.0/9.0) * (fahr-32.0)));
```
Then, I had to read more on Rust's formatting syntax to get it to give me the same right-justified, tabbed output as we had in C. At first pass, I think the C syntax here make more sense but I can see Rust's formatting syntax being more powerful in the long run. By the way, it's really nice not to have to a) declare variables in advance as we had to in C, and b) Rust's type-inference lets us not have to worry manually describing the types of our variables either (at least not yet).

main.c
```c
#include <stdio.h>

main() {
	float fahr, celsius;
	int lower, upper, step;

	lower = 0;
	upper = 300;
	step = 20;

	fahr = lower;
	while (fahr <= upper) {
		celsius = (5.0/9.0) * (fahr-32.0);
		printf("%3.0f %6.1f\n", fahr, celsius);
		fahr = fahr + step;
	}
}
```

main.rs
```rust
fn main() {
	let (lower, upper, step) = (0.0, 300.0, 20.0);

	let mut fahr = lower;
	while fahr <= upper {
		let celsius = format!("{:.*}", 1, ((5.0/9.0) * (fahr-32.0)));
		println!("{:>3} {:>6}", fahr, celsius);
		fahr += step;
		
	}
}
```
### 1.3 The For Statement

This part got a little interesting because I had to switch to Rust nightly just to be able to change the step size of the for loop iterator. After getting all that sorted, and adding #![feature(step_by)] to my code I was able to get the equivalent output as the C exercise using only a for loop. This also required me to manually cast fahr as a float in order to perform the Fahrenheit to Celsius conversion.

main.c
```c
#include <stdio.h>

main() {
	int fahr;

	printf("Fahrenheit\tCelsius\n");
	for (fahr = 0; fahr <= 300; fahr = fahr + 20) {
		printf("%3d %18.1f\n", fahr, (5.0/9.0) * (fahr-32.0));
	}
}
```

main.rs
```rust
#![feature(step_by)]

fn main() {
	println!("Fahrenheit {:^18}", "Celsius");
	for fahr in (0..300).step_by(20) {
		println!("{:>3} {:>18}", fahr, format!("{:.*}", 1, ((5.0/9.0) * (fahr as f64-32.0))));
	}
}
```