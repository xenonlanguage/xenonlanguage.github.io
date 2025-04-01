+++
draft = true
title = 'Lexical Structure'
+++

## Lexical Structure

The **Lexical Structure** of Xenon is the way the [*lexer*](https://crates.io/crates/xenon-lexer) handles the code given to it. The [lexer](https://crates.io/crates/xenon-lexer) is the first step in the compilation process. It takes raw text and transforms it into a series of tokens.

### Tokens

Tokens (as defined by the [xenon-lexer](https://crates.io/crates/xenon-lexer) crate) are a data structure consisting of a value, a type, and a line number.

```rust
pub struct Token {
    pub value: String,
    pub ttype: TokenType,
    pub line: usize
}
```