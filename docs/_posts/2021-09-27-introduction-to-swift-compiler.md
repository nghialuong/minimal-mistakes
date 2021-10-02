---
title: "Introduction to Swift Compiler"
tags:
  - swift
  - compiler
---

In my current team, a small iOS team that has 10 people, we are running sessions called `bi-weekly iOS sharing topics`, and luckily, it's been working the same way of its name. Last week was my turn. Before that, I had not had any idea of the topic I was supposed to share. Then, one good day, my super young talented colleague suggested a topic about `Swift compiler`. And now I ended up having a sharing session about it and this post as well.

We all know, `Swift` is a compiled language (I don't want to talk more about complied or interpreted language right now right here, there are tons of references that you can look up on the Internet), which means, it needs a `compiler`. Let's give a bit intro about it.

# Compiler's job

According to [wikipedia](https://en.wikipedia.org/wiki/Compiler):
> a compiler is a computer program that translates computer code written in one programming language (the source language) into another language (the target language)

For example, from a `.swift` file we wrote, once it is compiled, the output of the compiling process will become in the forms of machine code that a specific processor can read, like x86, ARM, …

# Compiler in general

Compiler in theory has 6 steps, the input is the source code, and output is machine code. The process runs serially meaning the output of the current step will become the input of the next step.
I won't go into details of each step, or what it does inside every step. Again, you can goole them out. Here are the steps:

- Lexical Analysis
- Syntax Analysis
- Semantic Analysis
- Intermediate Code Generation
- Code Optimization
- Code Generation

So, what's special about **Swift compiler**, let's dive into it.

# Swift compiler

`Swift compiler` not only has 6 phases above, but it also adds some more interesting layers. It basically has 2 parts: **front-end** and **back-end**. Here is the demonstration.

![](https://github.com/nghialuong/nghialuong.github.io/blob/gh-pages/docs/assets/images/swift_compiler_front_back_ends.jpg?raw=true)

Let's see what is going on at these two.

## Swift compiler front-end

![](https://github.com/nghialuong/nghialuong.github.io/blob/gh-pages/docs/assets/images/swift_compiler_pipeline.jpg?raw=true)

After the `swift` file is passed into swift compiler - which is `swiftc`, it will do the job. `Lexical Analyzer` and `Syntax Analyzer` consecutively will be put in place, and the output of the 2 steps is [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree). Next, the AST will be passed into the `Sematic analyzer`, and again, being analyzed. Now, is an interesting part. All the output will be passed into `SILGen`, so what is it and the thing named `SIL` as well?

### Swift Intermediate Language (SIL)

Apple engineers teams have been working well in order to make the swift compiler better compare to the Objective C compiler. They created SIL, which is the intermediate code representation between AST and LLVM IR. Before SIL, it's impossible to implement some high-level analysis, reliable diagnostic, and optimization for which neither AST nor LLVM IR layers. So, SIL is a nice and gentle solution for that. 

The process of SIL works in 2 steps, according to [Apple's document](https://github.com/apple/swift/blob/main/docs/SIL.rst)

```
- The SILGen module generates raw SIL from an AST.
- A series of Guaranteed Optimization Passes and Diagnostic Passes are run over the raw SIL both to perform optimizations and to emit language-specific diagnostics. - These are always run and produce canonical SIL.
- General SIL Optimization Passes optionally run over the canonical SIL to improve performance of the resulting executable.
```

After all, the output will be passed into `IRGen`, which basically generates LLVM IR. And, again, what is `LLVM IR`?

### LLVM and LLVM IR

LLVM is a big project, provides a toolkit for building compilers. Its job is to convert instruction into a form that can be read and executed by a computer, also porting the outputted code to multiple platforms and architectures. [Here](https://llvm.org/) if you want more details.

In Swift compiling pipeline, a part of if, `LLVM core` is used as a part of back-end. The back-end is using LLVM IR to be a consistent form of input while using `LLVM`. `IR` stands for Intermediate representation. Doing this, let's say when we want to create a new front-end (compiler), we don't need to re-invent the backend part, the only requirement here is that the new compiler needs to have the output in the form of LLVM IR.

# Conclusion

So we just have been through the Swift compiler pipeline, and its special components like SIL, LLVM IR. Now you have a basic idea of how the Swift compiler works. Still recommend you get familiar with the part of `Compiler in general` first, then it would make the after part so much easier. Hope you find something interesting reading this.

Happy learning! 💻
