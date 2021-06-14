# Builtin functions

As mentioned in the introduction, Mil's built-in functions just wrap
the MelVM opcodes. Below is a full spec of function names and corresponding
opcodes.

As a general pattern, a Mil function takes inputs in the same order they are
read in the VM. So `(- 5 2)` compiles to opcodes:
`[PUSHI(5) PUSHI(2) SUB]` which results in integer `3` being pushed to the
stack. Similarly, `(v-concat [1] [2])` results in a vector `[1 2]` on the stack.

You may notice that control flow and memory (heap) operations are not mentioned
here. Instead of being exposed directly, they are coupled into language
abstractions. See the next section on [] for more info.

## Arithmetic
Mil | Opcode
------ | ------
+ | ADD
- | SUB
* | MUL
/ | DIV
% | REM

## Logical
Mil | Opcode
------ | ------
and | AND
or | OR
xor | XOR
not | NOT
= | EQL
< | LT
\> | GT

## Cryptography
Mil | Opcode
------ | ------
hash | HASH
sigeok | SIGEOK

## Vectors
Mil | Opcode
------ | ------
v-get | VREF
v-concat | VAPPEND
v-nil | VEMPTY
v-len | VLENGTH
v-slice | VSLICE
v-from | VSET
v-push | VPUSH
v-cons | VCONS

## Bytestrings
Mil | Opcode
------ | ------
b-get | BREF
b-concat | BAPPEND
b-nil | BEMPTY
b-len | BLENGTH
b-slice | BSLICE
b-from | BSET
b-push | BPUSH
b-cons | BCONS

## Type conversions
Mil | Opcode
------ | ------
u256->bytes | ITOB
bytes->u256 | BTOI

