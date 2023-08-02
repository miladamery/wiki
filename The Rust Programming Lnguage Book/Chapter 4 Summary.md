
<h2>What Is Ownership?</h2>
Ownership is a set of rules that govern how a Rust program manages memory.
Some languages have garbage collection that regularly looks for no-longer-used memory as the program runs; in other languages, the programmer must explicitly allocate and free the memory. Rust uses a third approach: memory is managed through a system of ownership with a set of rules that the compiler checks.

<h2>THE STACK AND THE HEAP</h2>
All data stored on the stack must have a known, fixed size. Data with an unknown size at compile time or a size that might change must be stored on the heap instead.

<p>The heap is less organized: when you put data on the heap, you request a certain amount of space. The memory allocator finds an empty spot in the heap that is big enough, marks it as being in use, and returns a pointer, which is the address of that location.
Pushing values onto the stack is not considered allocating.
Because the pointer to the heap is a known, fixed size, you can store the pointer on the stack.</p>

Pushing to the stack is faster than allocating on the heap. 
Accessing data in the heap is slower than accessing data on the stack.

When your code calls a function, the values passed into the function (including, potentially, pointers to data on the heap) and the function’s local variables get pushed onto the stack. When the function is over, those values get popped off the stack.

Keeping track of what parts of code are using what data on the heap, minimizing the amount of duplicate data on the heap, and cleaning up unused data on the heap so you don’t run out of space are all problems that ownership addresses.


<h2>Ownership Rules</h2>
1. Each value in Rust has an owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

<h2>Variable Scope</h2>
<b>A scope is the range within a program for which an item is valid.</b>
The variable is valid from the point at which it’s declared until the end of the current scope.

<h2>String Type</h2>
```rust
let s = String::from("hello");
s.push_str(", world!"); // push_str appends a literal to a String
```

In the case of a string literal, we know the contents at compile time, so the text is hardcoded directly into the final executable.

<h2>Double Free Error</h2>
```rust
let s1 = String::from("hello"); 
let s2 = s1;
// From here s1 is no longer valid.
```
When `s2` and `s1` go out of scope, they will both try to free the same memory. This is known as a double free error and is one of the memory safety bugs.

To ensure memory safety, after the line `let s2 = s1;`, Rust considers `s1` as no longer valid.

<h2>What is <b>Move</b> in Rust</h2>
If you’ve heard the terms shallow copy and deep copy while working with other languages, the concept of copying the pointer, length, and capacity without copying the data probably sounds like making a shallow copy. But because Rust also invalidates the first variable, instead of being called a shallow copy, it’s known as a <b>move</b>.

<h2>Variables and Data Interacting with Clone</h2>
If we do want to deeply copy the heap data of the String, not just the stack data, we can use a common method called clone.

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
println!("s1 = {s1}, s2 = {s2}"); // Completely valid
```

<h2>Stack-Only Data: Copy</h2>
Rust has a special annotation called the `Copy` trait that we can place on types that are stored on the stack, If a type implements the `Copy` trait, variables that use it do not move, but rather are trivially copied.

as a general rule, any group of simple scalar values can implement `Copy`.

Tuples, if they only contain types that also implement `Copy`.

<h2>Ownership and Functions</h2>
Passing a variable to a function will move or copy, just as assignment does.
Returning values can also transfer ownership.
Rust has a feature for using a value without transferring ownership, called `references`.


<h2>References and Borrowing</h2>
A `reference` is like a pointer in that it’s an address we can follow to access the data stored at that address.
Unlike a pointer, a `reference` is guaranteed to point to a valid value of a particular type for the life of that `reference`.
```rust
fn main() {
	let s1 = String::from("hello");
	let len = calculate_length(&s1);

	println!("The length of '{s1}' is {len}."); 
}
fn calculate_length(s: &String) -> usize {
	s.len()
}
```
These ampersands represent references.

When functions have references as parameters instead of the actual values, we won’t need to return the values in order to give back ownership, because we never had ownership.

<b>We call the action of creating a reference borrowing.</b>
Just as variables are immutable by default, so are references. We’re not allowed to modify something we have a reference to.


<h2>Mutable References</h2>
```rust
fn main() {
	let mut s = String::from("hello");
	change(&mut s);
}

fn change(some_string: &mut String) {
	some_string.push_str(", world");
}
```
First we change s to be `mut`. Then we create a mutable reference with `&mut` s where we call the change function, and update the function signature to accept a mutable reference with `some_string`: `&mut` String. This makes it very clear that the change function will mutate the value it borrows.
<b>Mutable references have one big restriction: if you have a mutable reference to a value, you can have no other references to that value.</b>

The benefit of having this restriction is that Rust can prevent data races at compile time. A data race is similar to a race condition and happens when these three behaviors occur:
1. Two or more pointers access the same data at the same time.
2. At least one of the pointers is being used to write to the data.
3. There’s no mechanism being used to synchronize access to the data.
Rust prevents this problem by refusing to compile code with data races!

Whew! We also cannot have a mutable reference while we have an immutable one to the same value.
Users of an immutable reference don’t expect the value to suddenly change out from under them! However, multiple immutable references are <u>allowed</u> because no one who is just reading the data has the ability to affect anyone else’s reading of the data.

```rust
let mut s = String::from("hello");

let r1 = &s; // no problem let r2 = &s; // no problem println!("{r1} and {r2}");

// Variables r1 and r2 will not be used after this point.
let r3 = &mut s; // no problem println!("{r3}");
```
The scopes of the immutable references r1 and r2 end after the `println!` where they are last used, which is before the mutable reference r3 is created. These scopes don’t overlap, so this code is allowed: the compiler can tell that the reference is no longer being used at a point before the end of the scope.

<h2>Dangling References</h2>
A dangling pointer—a pointer that references a location in memory that may have been given to someone else. In Rust, by contrast, the compiler guarantees that references will never be dangling references: if you have a reference to some data, the compiler will ensure that the data will not go out of scope before the reference to the data does.

<h2>The Rules of References</h2>
1. At any given time, you can have either one mutable reference or any number of immutable references.
2. References must always be valid.

<h2>The Slice Type</h2>
There are slices too.