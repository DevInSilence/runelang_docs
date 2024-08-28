## Simplang-Language Design [2024-08-28]

I was not able to come up with a better name for the language, so we are just going to call it Simplang (Simpl(e)-language). We are going to cover the syntax and semantics of the language in this chapter.

## Syntax

I wanted to take something which is not too complicated to lex and parse, not too many rules, but still be different from other languages. I decided to borrow the syntax from C-Language. It's important to note that we might change the syntax as we go along. The language should be strongly typed, but for simplicity, we will probably not have type inference. As I said before, this might change as we go along.

## Types

I'm planning to support different types in the language. Here is a list of types that I'm considering:

- Primitive types:
  - `int`
  - `float`
  - `char`
  - `bool`
- Derived types:
  - `array`

We will add more types as we go along. Especially around pointers.

## Pointers

Pointers are a bit tricky to implement. We will use pointers for arrays, strings, and other data structures. The biggest issue is when we get to type checking. The best approach would be to either prohibit the direct use of pointers or to trust the programmer to use them correctly. We will probably implement pointer with loose type checking and make it a bad practice to use them.

## Mutability

One of the things a modern programmer loves is immutability by default. We will probably make variables immutable by default and provide a way to make them mutable. This is a good practice and will help us avoid a lot of bugs.

## Syntax

I just collected some ideas for the syntax.

```simplang
# A comment starts with a hash and goes until the end of the line (EOL)

# Global variables are declared outside of any scope and are globally accessible
a: int = 10
b: mut int = 20

# Pointers are declared with the `*` symbol
c: *int = &a

# Functions are declared with parentheses instead of colons
add(a: int, b: int) int {
  return a + b
}

# external functions like print are imported by declaring them without a body
print(a: int) void

# The main function is the entry point of the program
main() {
  print(add(a, b))
}
```

This is just a rough idea of what the syntax might look like. We will probably change it as we work on the language.
