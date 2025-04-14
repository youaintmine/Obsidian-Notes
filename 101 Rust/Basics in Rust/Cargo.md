Cargo is the rust's build system and package manager.
 - It builds our code
 - download the libraries.
 - and builds those libraries

- Creating a new project with Cargo
	- cargo new hello_cargo
	- cd hello_cargo it, ls will show us
	  Cargo.toml and a src with main.rs inside.
	- It looks like this in toml file
```
	  [package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]

```

Understanding toml

1. The [package] is a section, that indicates that the following statements are configuring a package.
2. [dependencies] is the section, that takes in dependencies