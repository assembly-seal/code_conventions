

# SERVER CODE CONVENTIONS

This guide will cover the expectations of code posted and discussed on the server. These conventions are put in place for the benefit of all who read your code. Having a unified system allows code to be more easily understood by others. We ask that you follow this guide as closely as possible. If you think that something in this guide should be changed then please let us know (this does not include stylistic preferences). Thank you for your cooperation!

## Table of Contents

1. [Introduction](#introduction)
	- [Server Focus](#server-focus)
	- [Symbol Table](#symbol-table)
2. [Style Guide](#style-guide)
	- [Program Order](#program-order)
	- [Sections](#sections)
	- [Data](#data)
	- [Labels](#labels)
	- [Instructions](#instructions)
	- [Grouping](#grouping)


## Introduction

### Server Focus

For the time being, this server is centered around x86_64 assembly. Code written and posted should try to be:
- Written with the x86_64 instruction set
- Written for Linux
- Written for the [FASM](https://flatassembler.net/) Assembler

### Symbol Table

In this guide, certain symbols will be used in order to more clearly differentiate white space characters. The following table will show the symbols and what they represent.

| Symbol | Meaning  |
|:------:|----------|
| **→**  | tab      |
| **᛫**  | space		|
| **⏎**  | newline  |

## Style Guide

### Program Order

Programs should be written in the following order:
1. format declaration
2. entry point declaration
3. includes, macros, definitions
4. read only data (`.rodata`)
5. writable data
	- `.data`
	- `.bss`
6. code (`.text`)

If there is no need for the entry point to be labeled, it can be declared at the beginning of the code section with `entry $`.

### Sections

There is no blank line required between the format line and the entry line, nor between includes or definitions. However, there must be a blank line between each section, and there must also be one after each segment is declared. The only white space that should be used for format, entry, includes, segments, etc is a space character. They should not be indented, nor should tabs be in between words.

```asm
; format᛫ELF64᛫executable 3⏎
; entry᛫_start⏎
; ⏎
format ELF64 executable 3
entry _start

; include᛫'unistd64.inc'⏎
; ⏎
include 'unistd64.inc'

; segment᛫readable⏎
; ⏎
segment readable

; segment᛫readable᛫writeable⏎
; ⏎
segment readable writeable

; segment᛫readable᛫executable⏎
; ⏎
segment readable executable

_start:
```

### Data

Data names should be all lowercase, and written in snake case. Underscores are also allowed at the beginning/end of names. Names should be offset with one tab, and data definition should be aligned to one tab past the longest name in the section. There should be one space after the data definition mnemonic and data should have no spaces after commas. Labels can be used for structures when it helps with the programs readability. These rules apply to sections `.rodata`, `.data`, and `.bss` alike.

```asm
segment readable writeable

;→name→→→→→→db᛫0
;→long_name→db᛫"Hello",0xA
	name		db 0
	long_name	db "Hello",0xA	; newline

struc:
	bit		db "byte"
	piece		db "nibble"
```

### Labels

The rules for naming labels are the same as the ones for [data names](#data). Labels should be left aligned and should be on their own separate line. There should not be a blank line after labels, however there can and often should be one above them.

```asm
_start:
	mov	rax, 0
	mov	rdi, 0
	mov rsi, input
	mov rdx, 2
	syscall

	cmp byte [input], 48	; "0"
	jne _start

exit:
	mov rax, 60
	xor rdi, rdi
	syscall
```

### Instructions

Instructions should be offset with one tab, and so should the first operand. The first operand should be followed by a comma, a space, and then the second operand.

```asm
;→mov→rax,᛫1
	mov	rax, 1
```

### Grouping

Groups of instructions should be separated by blank lines. Grouping should be based on instructions that have a common goal. However, which instructions are grouped together is ultimately decided by the developer.

```asm
_start:
	; all instructions for sys_write are grouped
	mov	rax, 1
	mov	rdi, 1
	mov	rsi, msg
	mov	rdx, len
	syscall

	; all instructions for sys_read are grouped
	mov rax, 0
	mov	rdi, 0
	mov	rsi, input
	mov rdx, 2
	syscall
	
	; all instructions for branching based on input are grouped
	cmp byte [input], 48	; "0"
	jl some_label
	jmp some_other_label
```
