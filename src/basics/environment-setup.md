## Environment Setup

We will be using `Rust 1.81.x` nothing else just Rust so you can install it directly on your host machine or use somekind of container or VM to run it. You can install Rust by following the instructions on the [official Rust website](https://www.rust-lang.org/en-US/install.html) or by using curl:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## LLVM

The LLVM setup is very important. I will document it here but because I want to write this book step by step I will not install it right now. I will install it when I need it.

## Platform

I'm working mainly on Unix based systems which means paths as well as library installations might differ on your system. I will try to provide as much information as possible.

## For Windows Users

You can enable the `Windows Subsystem for Linux (WSL)` optional feature and install a Linux distribution like Ubuntu from the Microsoft Store. This will allow you to run Linux commands and tools on your Windows machine. You can also use a virtual machine or a container to run Linux.

## Editor

I constantly switch between VSCode and RustRover. If you want a free and open source editor, I recommend VSCode. If you want a Rust specific editor, I recommend RustRover by JetBrains. You can also use Vim, Emacs, or any other editor you like.
