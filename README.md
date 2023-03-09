# code_conventions
Server Code Conventions and Style Guide

**SERVER CODE CONVENTIONS**

__**Style Guide**__

__Instructions__ will be written in the following style: `mnemonic [tab] operand1, [space] operand2`
The mnemonic will be lowercase and followed by a single tab, then the first operand, a comma, a space, and then the second operand

```diff
+CORRECT:
```
```assembly
; mnemnoic is followed by a tab
; there is no space between the first operator and the comma
; there is a space after the comma
mov  rax, 1
```
```diff
-INCORRECT:
```
```assembly
mov rax,1
```

__Labels__ will be written in lower case with underscores separating words. Underscores at the beginning and end of labels are also optional. Labels should take up their own line, and their should be no empty lines in between the label and the code.

```diff
+CORRECT:
```
```assembly
label:
  mov  rax, 1
```
```diff
-INCORRECT:
```
```assembly
label:  mov  rax, 1

label:

  mov  rax, 1
```

__Indentation__ should be done with tabs. Every line of code should be indented one tab out from whatever label it falls under. This applies to labels themselves as well.

```diff
+CORRECT:
```
```assembly
_start:
  mov  rax, 1

  label:
    mov  rax, 2
```
```diff
-INCORRECT:
```
```assembly
_start:
mov  rax, 1

label:
mov  rax, 2
```
