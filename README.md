> # 📖 La programmation asynchrone avec Rust
>
> **🌐 This is the french translation of the book "Asynchronous Programming in Rust"**
>
> [👓 Click here to read this translated book online](https://jimskapt.github.io/async-book-fr/)
>
> *[🔗 Click here to go to the English Book repository](https://rust-lang.github.io/async-book/)*
>
> Translations are inside [`/FRENCH/`](https://github.com/Jimskapt/async-book-fr/tree/french-release/FRENCH)
> folder. Everything else should be remaining as the English Book *(except some
> necessary files, like this README.md)*.
>
> Want to help to translate ?
> Please read the file
> [/FRENCH/CONTRIBUTING.md](https://github.com/Jimskapt/async-book-fr/blob/french-release/FRENCH/CONTRIBUTING.md) !

# async-book
Asynchronous Programming in Rust

## Requirements
The async book is built with [`mdbook`], you can install it using cargo.

```
cargo install mdbook
cargo install mdbook-linkcheck
```

[`mdbook`]: https://github.com/rust-lang/mdBook

## Building
To create a finished book, run `mdbook build` to generate it under the `book/` directory.
```
mdbook build
```

## Development
While writing it can be handy to see your changes, `mdbook serve` will launch a local web
server to serve the book.
```
mdbook serve
```
