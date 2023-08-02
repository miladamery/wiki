<h2>Constant Limitations</h2>
1. You are not allowed to use `mut` with constants, They are always <b>immutable</b>.
2. Type of constant must be annotated.
3. Constants may be set only to a constant expression.
Rust's naming convention for constants is to use all uppercase with underscores between words.

<h2>Shadowing</h2>
You can declare a new variable with the same name as a previous variable. 
Rustaceans say that the first variable is shadowed by the second.
We can shadow a variable by using the same variable’s name and repeating the use of the let keyword.
We can change the type of the value but reuse the same name.

<h2>Scalar Types</h2>
Rust has four primary scalar types: <b>integers</b>, <b>floating-point numbers</b>, <b>Booleans</b>, and <b>characters</b>.

<h3>Integer Overflow</h3>
When you’re compiling in debug mode, Rust includes checks for integer overflow that cause your program to panic at runtime if this behavior occurs.
When you’re compiling in release mode with the --release flag, Rust does not include checks for integer overflow that cause panics. Instead, if overflow occurs, Rust performs two’s complement wrapping. In short, values greater than the maximum value the type can hold “wrap around” to the minimum of the values the type can hold. In the case of a u8, the value 256 becomes 0, the value 257 becomes 1, and so on.

<h2>Compound Types</h2>
Rust has two primitive compound types: tuples and arrays.

<h3>Tuple</h3>
Tuples have a fixed length: once declared, they cannot grow or shrink in size.
The first index in a tuple is 0.
The tuple without any values has a special name, unit. This value and its corresponding type are both written () and represent an empty value or an empty return type. Expressions implicitly return the unit value if they don’t return any other value.

```rust
fn main() {
	let tup: (i32, f64, u8) = (500, 6.4, 1); 
}
```

<h3>Array</h3>
Arrays are useful when you want your data allocated on the stack rather than the heap.

```rust
fn main() {
	let a = [1, 2, 3, 4, 5]; 
}
```

Defining array:
```rust
let a: [i32; 5] = [1, 2, 3, 4, 5]; /*[i32; 5] first element is array element types and second one is length of array*/
```

<h2>Functions</h2>
Rust code uses snake case as the conventional style for function and variable names.

```rust
fn another_function() { 
	println!("Another function.");
}

fn five() -> i32 { 
	5 /*Notice that ; is missing and its correct because this is an    
	    expression*/
}
```

In function signatures, you must declare the type of each parameter.

<h2>Statements and Expressions</h2>
Rust is an expression-based language.
Statements are instructions that perform some action and do not return a value.
Expressions evaluate to a resultant value.

```rust
fn main() {
	let y = {
		let x = 3;
		x + 1 
	};
	println!("The value of y is: {y}"); 
}
```

<h2>Control Flow</h2>
<h3>If Expression</h3>
```rust
fn main() { 
	let number = 3;

	if number < 5 { 
		println!("condition was true");
	} else {
		println!("condition was false"); 
	}
}

fn main() { 
	let number = 6;

	if number % 4 == 0 {
		println!("number is divisible by 4"); 
	} else if number % 3 == 0 {
		println!("number is divisible by 3"); 
	} else if number % 2 == 0 {
		println!("number is divisible by 2"); 
	} else {
		println!("number is not divisible by 4, 3, or 2"); 
	}
}

fn main() {
	let condition = true;
	let number = if condition { 5 } else { 6 }; // If is an expression
	println!("The value of number is: {number}"); 
}
```

<h3>Loop Expression</h3>

```rust
fn main() {
	loop { 
		println!("again!");
	} 
}

fn main() {
	let mut counter = 0;

	let result = loop {
		counter += 1;
		if counter == 10 { 
			break counter * 2; // Returns value from loop
		} 
	};

	println!("The result is {result}"); 
}
```

If you have loops within loops, break and continue apply to the innermost loop at that point. You can optionally specify a loop label on a loop that you can then use with break or continue to specify that those keywords apply to the labeled loop instead of the innermost loop. <b>Loop labels must begin with a single quote</b>.

```rust
fn main() {
	let mut count = 0;
	'counting_up/*Loop label*/: loop {
		println!("count = {count}");
		let mut remaining = 10;
		loop {
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

<h3>While Expression</h3>
```rust
fn main() {
	let mut number = 3;
	while number != 0 {
		println!("{number}!");
		number -= 1;
	}
	println!("LIFTOFF!!!");
}
```

<h3>For Expression</h3>

```rust
fn main() {
	let a = [10, 20, 30, 40, 50];
	for element in a {
		println!("the value is: {element}");
	}
}

fn main() {
	for number in (1..4).rev() { /*(1..4) is a range*/
		println!("{number}!");
	}
	println!("LIFTOFF!!!");
}
```
