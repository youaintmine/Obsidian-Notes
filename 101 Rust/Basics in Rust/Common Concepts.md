Be default, variables are immutable.

```
fn main() {
    let x = 5;
    println!("The value of x is: {x}");
    x = 6;
    println!("The value of x is: {x}");
}
```

We'll recieve an error, regarding immutability.

But we can convert them to mutable. 
by adding mut

e.g let mut x = 5;

#### Constants
- Const are non-immutable anyways.
  const DAYS_IN_MS = 864000.
- Constants are valid for the entire lifetime of the program.

#### Shadowing
We can declare a new variable, with the same name. as previous one. 
But the first variable is shadowed by the second.

```
fn main() {
	let x = 5;
	let x = x + 1; 
	//This is creating a new variable of x.
	{
		let x = x * 2;
		//Value of x = 12
	}
	//value of x 6.
}
```

Also, we're not allowed to mutate a variables type.

