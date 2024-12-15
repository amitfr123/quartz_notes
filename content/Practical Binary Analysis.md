---
title: Practical Binary Analysis
draft: false
description: Summary of Practical Binary Analysis book by Dennis Andriesse.
---
## Anatomy of a binary
### The C Compilation Process
This chapter of the book describes a rough overview of the `gcc` compilation process.
1. The Pre-processing Phase - in this phase macros like `#define` and `#include` are resolved. To view this stage you can use the `-E` flag when using`gcc`.
2. The Compilation Phase - in this phase the code is compiled into assembly in a human readable format ie with symbolic information (like the usage of function names). To view the output of this phase you can use the `-S` flag when using`gcc`.
3. The Assembly Phase - in this phase object files (`ELF` format) are generated. The object files contain proper assembly that can be executed. To view the output of this phase you can use the `-C` flag when using`gcc`.
4. The Linking Phase - in this phase the object files are linked together to form a single executable/library.

### Symbols and Stripped Binaries
This chapter isn't super interesting. Here is a short summary of the relevant things.
- Symbols are artifacts the are emitted by the compiler to keep track of symbolic names relative to the relevant code (like function names).
- To view symbols in the `ELF` use the `--syms` flag when using `readelf`.
- Debug symbols exist like `DWARF` format and the `BDP` format.
- You can strip binaries by using `strip --strip-all /path/to/foo`.
- After stripping a file there still symbols in the `.dynsym` symbol table. This symbols are needed for dynamic linking.
- You can use the `--relocs` flag when using `readelf` to view the relocation symbols in the object file. This symbols contain information about unresolved functions that the linker still needs to take care of.

### Loading an Executing a Binary 
#### The Interpreter
The operating systems defers the work relevant to loading the binary and perform the necessary relocations to a user space program called the "Interpreter".
The interpreter loads the binary into its virtual address space, then performs dynamic linking operations like looking up dynamically linked libraries and mapping them into the virtual address space and do necessary relocations. Usually the resolving is done later when the dependency is called ([lazy linking](https://www.qnx.com/developers/docs/8.0/com.qnx.doc.neutrino.prog/topic/devel_Lazy_binding.html)). After the relocation the interpreter looks up the entry point and transfers control to it.
##### notes
- In Linux the interpreter is usually a shared library called `ld-linux.so`.
- In Windows the interpreter is a part of `ntdll.dll`.
* The `ELF` format contains a special section called `.interp` that contains the path to the interpreter.

## ELF Format
This diagram shows the different headers in the `ELF` format.
![[elf_file_overview.png]]
### Sections
This chapter just contains information about a bunch of sections.
#### `.init` and `.fini`
This sections contain executable code that act like the constructors and destructors of the program.  `.init` contains code that needs to run before transferring control to the main entry point. of the program. `.fini` contains code that needs to run after the the main program completes.
#### `.text`
Contains the main code of the program. The section contains user defined code and code that is automatically added by the compiler. For example `gcc` will add functions like `_start`, `register_tm_clones`, `frame_dummy`.
#### `.rodata`, `data` and `.bss`
1. `.rodata` - is a readonly section that contains constant values.
2. `.data` - contains writable variables that are initialized.
3. `.bss` - represents uninitialized variables. because this section doesn't need to occupy space on the disk it is marked as `SHT_NOBITS` instead of `SHT_PROGBITS` like `data` and `.rodata`.
#### `.plt`,`.got` and `.got.plt`
The `.plt` section contains executable code that reads an entry in the `GOT` sections and jumps to the correct function for the dynamic linking logic.
`.got` and `.got.plt` can be treated the same way and they serve the same function, hold the address to the virtual address of the function.
The use case for this will be explained more in depth later.