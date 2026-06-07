# How to write a compiler ?

This series of blogs acts as an analysis and deep dive into compiler design.

> Currently I am building EEL. EEL is eBPF Language, this series of blogs consists of all the things I learned throughout the process of building it.

---

## What is a compiler?

A compiler is a program that translates source code written in one language into another representation, usually machine code or an intermediate form that can be executed by a computer.

Modern compilers are made up of several different pieces which come together as a puzzle to make a compiler.

These pieces are:

* Lexer
* Parser
* Semantic Analyzer
* IR Generator
* Optimizer
* Code Generator
* Assembler
* Linker

We will analyze each of these one by one in great detail.

```text
Source Code
     ↓
   Lexer
     ↓
  Tokens
     ↓
  Parser
     ↓
    AST
     ↓
Semantic Analysis
     ↓
     IR
     ↓
 Optimizer
     ↓
Code Generator
     ↓
 Assembler
     ↓
   Linker
     ↓
 Executable
```

Throughout the blogs we will take following pseudocode as reference:

```text
fn add(a,b){
    price = 10;
    total = price + 42;
    print(total);
}
```
