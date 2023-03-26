

# SERVER CODE CONVENTIONS

This guide will cover the expectations of code posted and discussed on the server. These conventions are put in place for the benefit of all who read your code. Having a unified system allows code to be more easily understood by others. We ask that you follow this guide as closely as possible. If you think that something in this guide should be changed then please let us know (this does not include stylistic preferences). Thank you for your cooperation!

## Table of Contents

1. [Introduction](#introduction)
	- [Server Focus](#server-focus)
2. [Style Guide](#style-guide)
	- [Program Order](#program-order)
	- [Sections](#sections)
	- [Data](#data)
	- [Labels](#labels)
	- [Instructions](#instructions)
	- [Grouping](#grouping)
	- [Comments](#comments)
3. [Best Practices](#best-practices)
	- [Magic Numbers](#magic-numbers)
	- [Syscalls](#syscalls)
	- [Commenting](#commenting)

## Introduction

### Server Focus

For the time being, this server is centered around x86_64 assembly. Code written and posted should try to be:
- Written with the x86_64 instruction set
- Written for Linux
- Written for the [FASM](https://flatassembler.net/) Assembler

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

There is no blank line required between the format line and the entry line, nor between includes or definitions. There should be a blank line between each section, and also one after each segment is declared. The only whitespace that should be used for format, entry, includes, segments, etc is a space character. They should not be indented, nor should tabs be in between words.

```asm
format ELF64 executable 3	; format declaration
entry _start			; entry point declaration

include 'unistd64.inc'		; includes, macros, definitions

segment readable		; .rodata

segment readable writeable	; .data + .bss

segment readable executable	; .text

_start:
```

### Data

Data names should be all lowercase, and written in snake case. Underscores are also allowed at the beginning/end of names. Names should be offset with one tab, and data definition should be aligned to one tab past the longest name in the section. There should be one space after the data definition mnemonic and data should have no spaces after commas. Labels can be used for structures when it helps with the programs readability. These rules apply to sections `.rodata`, `.data`, and `.bss` alike.

```asm
segment readable writeable

	name		db 0
	long_name	db "Hello",0xA	; newline

struc:
	bit		db "byte"
	piece		db "nibble"
```

### Labels

The rules for naming labels are the same as the ones for [data names](#data). Labels should be on the very left and on their own separate line. There should not be a blank line after labels, however there can and often should be one above them.

```asm
_start:
	mov	rax, 0
	mov	rdi, 0
	mov	rsi, input
	mov	rdx, 2
	syscall

	cmp	byte [input], 48	; "0"
	jne	_start

exit:
	mov	rax, 60
	xor	rdi, rdi
	syscall
```

### Instructions

Instructions should be offset with one tab, and so should the first operand. Operands should be separated by a comma and a single space. There should be no extra spacing around operators. Type specifiers should be placed before the first operand, and there should be a single space between them.

```asm
	mov	rax, 1
	cmp	byte [addr+1], 1
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
	mov	rax, 0
	mov	rdi, 0
	mov	rsi, input
	mov	rdx, 2
	syscall
	
	; all instructions for branching based on input are grouped
	cmp	byte [input], 48	; "0"
	jl	some_label
	jmp	_start
```

### Comments

Comments are divided into two groups: 'group explanations' and 'line explanations'. (There are also comments which do not explain code, however the stylization of those comments is up to the developer). Group explanations should come right before a code group, with no lines of space in between. They can be multiple lines. Line explanations are a single line comment placed after an instruction. Line explanations should all be aligned to one tab past the longest instruction.

```asm
segment readable writeable

	name		db 0
	long_name	db "Hello",0xA	; newline

segment readable executable

	; prints out long_name
	; save value of rax before printing and restores it afterwards
	push	rax			; save rax value
	mov	rax, 1			; sys_write
	mov	rdi, 1			; stdio
	mov	rsi, long_name		; write long_name
	mov	rdx, 6			; write 6 bytes
	syscall				; call kernel
```

## Best Practices

### Magic numbers

Hex, Octal, and Decimal numbers may be used, however there should never be any magic numbers in your code. A magic number is a numeric literal that is used in code without any explanation of its meaning. Magic numbers are bad practice and can make your code confusing. You should always explain hard coded numbers with a comment.

```asm
	mov	rax, 48	; ascii code for "0"
```

### Syscalls

When specifying a syscall with a value in the `RAX` register, the value should either be a literal with a comment, or a constant value. The latter of the two is preferred. In either case, the naming should comply to the standard set in the [unistd64.inc](https://github.com/assembly-seal/code_conventions/tree/main/unistd64.inc) file.

```asm
	include 'unistd64.inc'
	
	; ...
	
	mov	rax, 60	; sys_exit
	mov	rax, sys_exit
```

### Commenting
