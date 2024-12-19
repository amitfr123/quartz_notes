---
title: Practical Binary Analysis
draft: false
description: Summary of Practical Binary Analysis book by Dennis Andriesse.
---
> [!info] Incomplete article head
# Anatomy of a binary
## The C Compilation Process
This chapter of the book describes a rough overview of the `gcc` compilation process.
1. The Preprocessing Phase - in this phase macros like `#define` and `#include` are resolved. To view this stage you can use the `-E` flag when using`gcc`.
2. The Compilation Phase - in this phase the code is compiled into assembly in a human readable format i.e. with symbolic information (like the usage of function names). To view the output of this phase you can use the `-S` flag when using`gcc`.
3. The Assembly Phase - in this phase object files (`ELF` format) are generated. The object files contain proper assembly that can be executed. To view the output of this phase you can use the `-C` flag when using`gcc`.
4. The Linking Phase - in this phase the object files are linked together to form a single executable/library. This phase also performs static relocations.

## Symbols and Stripped Binaries
This chapter isn't super interesting. Here is a short summary of the relevant things.
- Symbols are artifacts the are emitted by the compiler to keep track of symbolic names relative to the relevant code (like function names).
- To view symbols in the `ELF` use the `--syms` flag when using `readelf`.
- Debug symbols exist like `DWARF` format and the `BDP` format.
- You can strip binaries by using `strip --strip-all /path/to/foo`.
- After stripping a file there still symbols in the `.dynsym` symbol table. This symbols are needed for dynamic linking.
- You can use the `--relocs` flag when using `readelf` to view the relocation symbols in the object file. This symbols contain information about unresolved functions that the linker still needs to take care of.

## Loading an Executing a Binary 
### The Interpreter
The operating systems defers the work relevant to loading the binary and perform the necessary relocations to a user space program called the "Interpreter".
The interpreter loads the binary into its virtual address space, then performs dynamic linking operations like looking up dynamically linked libraries and mapping them into the virtual address space and do necessary relocations. Usually the resolving is done later when the dependency is called ([lazy linking](https://www.qnx.com/developers/docs/8.0/com.qnx.doc.neutrino.prog/topic/devel_Lazy_binding.html)). After the relocation the interpreter looks up the entry point and transfers control to it.
#### Notes
- In Linux the interpreter is usually a shared library called `ld-linux.so`.
- In Windows the interpreter is a part of `ntdll.dll`.
* The `ELF` format contains a special section called `.interp` that contains the path to the interpreter.

# ELF Format
[The android tool chain](https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/host/x86_64-linux-glibc2.7-4.6/+/jb-dev/sysroot/usr/include/elf.h) contains the header file `elf.h` that describes most (if not all) structures and macros relevant to the ELF format.
This diagram shows the different headers in the `ELF` format.
![[elf_file_overview.png]]
## Sections
This chapter just contains information about a bunch of sections.
### `.init` and `.fini`
This sections contain executable code that act like the constructors and destructors of the program.  `.init` contains code that needs to run before transferring control to the main entry point. of the program. `.fini` contains code that needs to run after the the main program completes.
### `.text`
Contains the main code of the program. The section contains user defined code and code that is automatically added by the compiler. For example `gcc` will add functions like `_start`, `register_tm_clones`, `frame_dummy`.
### `.rodata`, `data` and `.bss`
1. `.rodata` - is a read-only section that contains constant values.
2. `.data` - contains writable variables that are initialized.
3. `.bss` - represents uninitialized variables. because this section doesn't need to occupy space on the disk it is marked as `SHT_NOBITS` instead of `SHT_PROGBITS` like `data` and `.rodata`.
### `.plt`,`.got` and `.got.plt`
The `.plt` section contains executable code that reads an entry in the `GOT` sections and jumps to the correct function for the dynamic linking logic.
`.got` and `.got.plt` can be treated the same way and they serve the same function, hold the address to the virtual address of the function.
The use case for this will be explained more in depth later.

### `.rel.*` and `.rela.*`
This sections hold relocation entries used during the linking process. `.rela.*` sections hold an additional parameter `addend` whose value is added to the result of the relocation.
The use case for this will be explained more in depth later.

### `.dynamic`
This section contains a table of `tags` of different types. This tags hold important information that is used by the dynamic linker. This can information like: libraries it needs to link, version information, important pointers.

### `.init_array` and `.fini_array`
This sections are similar to `.init` and `.fini` but instead of holding a single function (which is usually supplied by your toolchain), this sections hold a table.
To create a custom destructors and constructors you can use the following attributes `_attribute__((constructor))` and `_attribute__((destructor))`. This attributes also receive a `priority` parameter that denotes the execution order.
It seems that [`atexit`](https://man7.org/linux/man-pages/man3/atexit.3.html) registered function is called before the destructors.
#### constructor example
Stolen from [arm developer](https://developer.arm.com/documentation/101754/0623/armclang-Reference/Compiler-specific-Function--Variable--and-Type-Attributes/--attribute----constructor-priority----function-attribute).
```C
#include <stdio.h>

void my_constructor1(void) __attribute__((constructor));
void my_constructor2(void) __attribute__((constructor(102)));
void my_constructor3(void) __attribute__((constructor(103)));
void my_constructor1(void) /* This is the 3rd constructor */
{                        /* function to be called */
    printf("Called my_constructor1()\n");
}
void my_constructor2(void) /* This is the 1st constructor */
{                         /* function to be called */
    printf("Called my_constructor2()\n");
}
void my_constructor3(void) /* This is the 2nd constructor */
{                         /* function to be called */
    printf("Called my_constructor3()\n");
}
int main(void)
{
    printf("Called main()\n");
}
```
The execution order will be:
```
Called my_constructor2()
Called my_constructor3()
Called my_constructor1()
Called main()
```
### `.shstrtab`
This section holds a table of null terminated strings for the section names.
### `.strtab` and `.symtab`
The `.strtab` section holds a table of null terminated strings for symbols the stored in `.symtab`. `.symtab` holds a table of symbols that associate a symbolic name with a section of code.
It should be noted that this sections are created and used during static compilation. Because they aren't used when executing and linking they are used for debugging and may be stripped.

### `.dynstr` and `.dynsym`
This sections are similar to `.strtab` and `.symtab`. The difference is that this sections hold information that is needed for dynamic linking and therefore they may not be stripped.