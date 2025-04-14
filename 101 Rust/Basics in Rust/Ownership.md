Ownership is a set of rules that govern how a rust program manages memory. 

- Stack operates on LIFO.
- Rules: 
	- Each value in rust has an owner.
	- only one owner at a time.
	- If owner goes out of scope, value will be dropped.

Also, string is stored on the heap.
String literal, knows the space string uses at compile time.
String is immutable, 
Second type is String, it manages data allocated on the heap and as such is able to store an amount of text unknown to us at compile time.

String literal creation: 
let s = String::from("hello");

Mutable

let mut s = String::from("hello");
s.push_str(", world");

- This ::from() allocates memory at runtime.

When a variable goes out of scope, Rust calls ==drop== function.

A string is made up of three parts, a pointer to the memory that holds the contents of the string, lenght and the capacity.

```
let s1 = String::from("hello");
let s2 = s1
```

![[string1 1.svg]]
But, this is what happens

![[string2.svg]]

Now, what rust does is invalidate s1, after assigning it to s2.
And hence example code 

```
let s1 = String::from("hello");
let s2 = s1

//Do something with s1
```


This above will return an error.

-- When we're creating s2, then s1 is being automatically dropped.

If you’ve heard the terms _shallow copy_ and _deep copy_ while working with other languages, the concept of copying the pointer, length, and capacity without copying the data probably sounds like making a shallow copy. But because Rust also invalidates the first variable, instead of being called a shallow copy, it’s known as a _move_.
### Clone
If we want to deeply copy the heap data of String, and not just the stack data, we can use a common method called clone.

```
let s1 = String::from("gre");
let s2 = s1.clone();
```


### Copy
Rust has a special annotation called the ==Copy== trait, that we can place on types that are placed on stack.
If a type implements a Copy trait, variables that use it do not move, but are copied, making them valid after assigning them to another variable.

- All the integer types, such as `u32`.
- The Boolean type, `bool`, with values `true` and `false`.
- All the floating-point types, such as `f64`.
- The character type, `char`.
- Tuples, if they only contain types that also implement `Copy`. For example, `(i32, i32)` implements `Copy`, but `(i32, String)` does not.


## Ownership and Function

Passing a variable to a function will move or just copy.

```
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // because i32 implements the Copy trait,
                                    // x does NOT move into the function,
    println!("{}", x);              // so it's okay to use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{some_string}");
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{some_integer}");
} // Here, some_integer goes out of scope. Nothing special happens.
```

How should I be able to let function let use a value but not take ownership. This is where references come in.

```
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

```

```
fn calculate_length(s: &String) -> usize { // s is a reference to a String
    s.len()
} // Here, s goes out of scope. But because s does not have ownership of what
  // it refers to, the value is not dropped.
```

So, if we're trying to change something we're borrowing.It throws an error.


### Mutable References.

```
fn main() {
	let mut s = String::from("hello");
	change(&mut s);
}

fn change(some_str: &mut String) {
	some_string.push_str(", world");
}

```

One big restriction is if mutable reference to a value, can have no other references to that value.
e.g this will fail.

let r1 = &mut s;
let r2 = &mut s;

Rust enforces a similar rule for combining mutable and immutable references. This code results in an error:

```
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    let r3 = &mut s; // BIG PROBLEM

    println!("{}, {}, and {}", r1, r2, r3);
```

```
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    println!("{r1} and {r2}");
    // variables r1 and r2 will not be used after this point

    let r3 = &mut s; // no problem
    println!("{r3}");

```

### Dangling References

