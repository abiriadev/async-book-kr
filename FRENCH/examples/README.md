# `/FRENCH/examples`

Here are the code that we translated. It is used in book code examples for
pedagogic purposes.

Please also take look to [`/FRENCH/examples-sources`][] folder !

## Example

Consider the following Markdown code, in
[`/FRENCH/src/ch02-00-guessing-game-tutorial.md`][] :

````markdown
```rust,ignore
{{#rustdoc_include ../examples/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:io}}
```
````

> It should import and show code from file ...
>
> [`/FRENCH/examples/ch02-guessing-game-tutorial/listing-02-01/src/main.rs`][]
>
> ... between `// ANCHOR: io` and `// ANCHOR_END: io` tags.

Potential processed result by `mdbook build` in the page which uses this
Markdown :

````markdown
```rust,ignore
use std::io;
```
````

<!-- LINKS : -->

[`/FRENCH/examples-sources`]:
https://github.com/Jimskapt/async-book-fr/tree/french-release/FRENCH/examples-sources

[`/FRENCH/src/ch02-00-guessing-game-tutorial.md`]:
https://github.com/Jimskapt/async-book-fr/blob/french-release/FRENCH/src/ch02-00-guessing-game-tutorial.md

[`/FRENCH/examples/ch02-guessing-game-tutorial/listing-02-01/src/main.rs`]:
https://github.com/Jimskapt/async-book-fr/blob/french-release/FRENCH/examples/ch02-guessing-game-tutorial/listing-02-01/src/main.rs
