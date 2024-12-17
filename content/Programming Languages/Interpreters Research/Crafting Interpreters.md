---
title: Crafting Interpreters
draft: false
description: Summary of Crafting Interpreters by Robert Nystrom.
---
> [!info] Incomplete article head
# Interpreter basics
## The components of an interpreter
1) **[[Lexical Analysis(Scanning)]]** - Taking a linear stream of characters and separating them to separate tokens.
2) **Parsing** - Taking a flat sequence of tokens and building a tree that mirrors the language syntax. This tree may be called by several names parse tree, abstract syntax tree (**AST**) or just trees. This stage is also the stage in which **syntax errors** are detected.
3) **Static analysis** - After parsing we know which expressions are nested in which but we don't know much more. This means we need to do stuff like:
	1. **Binding/Resolution** - For each identifier we need to find where it is defined. This is where **scopes** comes to play.
	2. **Type checking** - in statically typed languages we need to check if the operation between 2 identifier type is supported. For an example a*b make sense for integers but not for strings. This is where **type errors** are detected.
	3. Information for analysis can be saved as an **attribute** of the **AST** or be stored in lookup tables for example identifiers use a **symbol table** to match names to values. Another method is transforming the tree into a new data structure that is more convenient.
4) **Intermediate representation (IR)** - We may wish to transform our code into a new simpler and more easily managed representation. This means we can separate our interpreter into 2 parts. **Front end** that produces **IR** and a **back end** that takes **IR** and matches it into the target architecture.
5) **Optimization** - There are many methods for optimizing the code for example: constant folding.
6) **Code generation** - There are 2 main ways to generate code. We can either generate actual machine code that can actually run by itself or we can create virtual machine code that will run inside a **"VM"** a common name for it is byte-code.
7) **Virtual machine** - This stage is only relevant if we use byte-code. Again there are 2 options we can take the byte-code and implement a mini compiler for it. This again means that we will have a fully functioning binary or we can implement a **"Virtual Machine"** that emulates the our chip and executes the byte-code at runtime. The **VM** option is slower but it is much simpler and more easily portable.
8) **Runtime** - If we outputted machine code we can simply tell the operating system to load and run it and if we chose to go for the virtual machine option we need to startup the **VM** and load the program into it. For higher level programming languages we still use some kind of service/runtime to manage things like garbage collectors.

## Common shortcuts
* **Single pass compilers** - This is a technique that allows us to produce output code directly in the parser without allocating an **AST** or other **IR**. Older languages like **C** were designed this way because memory was very limited. This kind of compilers restrict the design of our language for example this is the limitation that forces functions in **C** to be declared above the calling function.
* **Tree walking interpreters** - This is a technique that allows us to begin executing code right after parsing it to an **AST**. This means that the program traverses the syntax tree one branch and a leaf at a time. This is common for student projects and little languages but not for general-purpose languages.
* **Transpilers** - Transpilers usually take a higher level programming languages and convert it to a lower (as low as the interpreter) language. They are also called **source-to-source compilers** or **trans-compilers**. This is a very popular method that is used to convert code to **C** (super popular low level system programming language) and code to **JavaScript** (the language used by modern browsers).
* **Just in time compiling (JIT)** - This means that during runtime we compile our code from source or byte-code to machine code. In the case of languages like **Java** or **C#** there is a matching runtime environment that does that **JVM** and **CLR**.

## Compiling vs interpreting
This is like compering fruits and vegetables. Where fruit is a botanical term and vegetables is a culinary term. For example **transpiling** is also compiling because compiling just means the process of converting from one language to another.
The main distinction is when someone says compiling the program translates code but doesn't run it and when they say interpreting they mean the program takes code and executes it.

# Programming language characteristics
* **Typing system** - dynamic vs static typing. 
* **Memory management** - automatic memory management (reference counting or tracing garbage collection) or manual memory management (malloc/free and new/destroy) or **borrow checking** (static analysis based memory management done at compile time).
* **Data types** - Even in dynamic programming languages there are type to variables like boolean integers and the mystical NULL/NIL.
* **Expressions** - There are several types of expressions like: 
	* Arithmetic expressions - binary operators like +, -, *, /.
	* Comparisons expressions - <, <=, >=, \==.
	* Logical operators expressions - !, &&, and, ||, or.
	* Precedence and grouping - () for arithmetic grouping.
	* Bit-wise and shifting - operators like ~, |, &, ^, >>.
* **Statements** - They produce an effect unlike expressions that produce a value. Statements are often grouped into blocks (scoping).  There are also expression statements that turn expressions to statements. Some languages don't use statements and treat everything as an expression for example "if" equals the value of its branches.
* **Variables** - Identifiers that represent data.
* **Control flow** - This means ifs and loops and also && || because they effect if the right expression is evaluated.
* **Functions** - functions are blocks (pieces) of code that can be called to cause some sort of effect.
* **Closures** - this are functions that hold on to references to surrounding variables. This is often conflated with first-class functions (functions you can use are values pass references to them and more).
* **Object oriented programming vs Functional programming** - This are 2 popular styles of programming language paradigms. **OOP** uses classes inheritance and encapsulate state and behavior. And functional separates state and behavior and focuses on function composition using pure functions (no hidden control flow/global state).
* **OOP classes vs prototypes** - Classes contains methods, inheritance chains and state. While prototypes don't use classes they only contain state and methods and the effect of something like inheritance is done via delegation to another object.
* **Standard library** - Every language come with a sizable preset of functions that are used for various things from string manipulation to IO operations and more.