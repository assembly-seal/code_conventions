# SERVER CODE CONVENTIONS

This guide will cover the expectations of code posted and discussed on the server. These conventions are put in place for the benefit of all who read your code. Having a unified system allows code to be more easily understood by others. We ask that you follow this guide as closely as possible. If you think that something in this guide should be changed then please let us know (this does not include stylistic preferences). Thank you for your cooperation!

Server Focus
------------

For the time being, this server is centered around x86_64 assembly. Code written and posted should try to be:
- Written with the x86_64 instruction set
- Written for Linux
- Written for the FASM Assembler

Style Guide
-----------



<ins>Instructions</ins> will be written in the following style: `mnemonic [tab] operand1, [space] operand2`
The mnemonic will be lowercase and followed by a single tab, the first operand, a comma, a space, and the second operand

```assembly
mov  rax, 1
```

<ins>Labels</ins> will be written in lower case with underscores separating words. Underscores at the beginning and end of labels are also optional. Labels should take up their own line, and their should be no empty lines in between the label and the code.

```assembly
label:
  mov  rax, 1
```

<ins>Indentation</ins> should be done with tabs. Every line of code should be indented one tab out from whatever label it falls under. This applies to labels themselves as well.

```assembly
_start:
  mov  rax, 1

  label:
    mov  rax, 2
```
