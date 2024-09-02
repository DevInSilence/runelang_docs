## Error Handling

In this section we will cover a basic error implementation. We will add a feature for extracting source code later we just need a basic error which we use for the lexer and parser. For this we need a new module in the `rune-core` crate.

### Module

First we need a new module for the error handling:
`runelang/rune-core/lib.rs`

```rust
pub mod error;
```

This time we don't need a whole folder we just accept the error.rs file in the `rune-core` crate.

### Error struct

Now we open this file and create a new struct called `CoreError`:

`runelang/rune-core/src/error.rs`

```rust
use core::fmt;

use crate::lexer::location::Location;

pub struct CoreError {
    pub loc: Option<Location>,
    pub message: String,
}

impl CoreError {
    pub fn new(loc: Option<Location>, message: String) -> Self {
        Self { loc, message }
    }
}

impl fmt::Display for CoreError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        if let Some(loc) = &self.loc {
            write!(
                f,
                "{}:{}:{}: {}",
                loc.filename, loc.line, loc.column, self.message
            )
        } else {
            write!(f, "{}", self.message)
        }
    }
}
```

If you take a look at the code we define a small struct which holds an optional `Location` which we use for the basic formating but later for enhanced errors. We also hold a `message` which is the error message we want to display. Finally we implement `fmt::Display` for the `CoreError` struct so we can print it using `println!("{}", error)`.

### Small lexer adjustment

Now with the error we create a `Vector` in the `lex` method which will hold all the lexical errors we encounter. We also need to adjust the `lex` method to return a `Result` instead of a `Token`. Finally in the main lex loop just for demonstration purposes we append an error for every character we encounter.

`runelang/rune-core/src/lexer/lexer.rs`

```diff
- pub fn lex(&mut self) -> Vec<Token> {
+ pub fn lex(&mut self) -> Result<Vec<Token>, Vec<CoreError>> {
+   let mut errs = Vec::new();

    while !self.is_eof() {
-     unimplemented!("Lexing loop")
+     errs.push(CoreError::new(None, "Not implemented".to_string()));
    }

-    self.tokens.clone()
+    if errs.is_empty() {
+        Ok(self.tokens.clone())
+    } else {
+        Err(errs)
+    }
}
```

Here the actual code:

```rust
pub fn lex(&mut self) -> Result<Vec<Token>, Vec<CoreError>> {
    let mut errs = Vec::new();

    while !self.is_eof() {
        errs.push(CoreError::new(None, "Not implemented".to_string()));
    }

    if errs.is_empty() {
        Ok(self.tokens.clone())
    } else {
        Err(errs)
    }
}
```

### Testing

Optionally we can add tests for the error handling. I just created one at this time which checks for the correct error formatting. You can take the code and put it into `runelang/rune-core/src/error.rs`.

```rust
#[cfg(test)]
mod tests {
    use super::*;
    use crate::lexer::location::Location;

    #[test]
    fn test_core_error_display() {
        let loc = Location::new("test.rn", 0, None, 1, 1);
        let error = CoreError::new(Some(loc), "Test error".to_string());
        assert_eq!(error.to_string(), "test.rn:1:1: Test error");
    }
}
```

### Source code

You can find the source code on the github repo under the tag [`errorHandling`](https://github.com/DevInSilence/runelang/tree/basicErrorHandling)
