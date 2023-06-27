# eZ80 Disassembler

Derived from `CE-Programming/zdis`

Design to disassemble a binary file containing eZ80 (or Z80) assembly code

Example command line:

```
ez80-dis --ez80 --start 0x45 --target 0x40000 --address --hex-dump
--compute-relative test.bin
```

To compile using clang:

```
clang -o ez80-dis ez80-dis.c zdis.c
```

You need a compiler that supports C11 in order to build `ez80-dis` (otherwise, just remove the `static_assert` usages).

The program has been enhanced versus the original:

- Allow for a target address that the program would be loaded. Previously this was assumed to be zero

- Print address for each line of assembler

- Include option for hex-dump so can see both the hex & assembly on the same line

- Can use decimal, hex or octal for command line parameters using standard C-format (leading 0x or 0X for hex, or leading 0 for octal)



- | Command line option | Meaning                                                           |
  | ------------------- | ----------------------------------------------------------------- |
  | --start             | address at which to start disassembly (relative to start of file) |
  | --end               | address at which to end disassembly (relative to start of file)   |
  | --target            | target address the program is designed to be loaded at            |
  | --address           | print address on each line of the output                          |
  | --hex-dump          | print hex dump on each line of the output                         |
  | --lowercase         | lowercase alpha                                                   |
  | --uppercase         | uppercase alpha                                                   |
  | --implicit-dest     | z80-style omitted dest (or a)                                     |
  | --explicit-dest     | ez80-style dest (or a,a)                                          |
  | --ez80              | assume adl = 1                                                    |
  | --z80               | assume adl = 0                                                    |
  | --suffix            | suffix immediates with base indicator (0FFh)                      |
  | --prefix            | prefix immediates with base indicator ($FF)                       |
  | --decimal           | base-10 immediates (255)                                          |
  | --hex               | base-16 immediates ($FF)                                          |
  | --mnemonic-space    | space after mnemonic                                              |
  | --mnemonic-tab      | tab after mnemonic                                                |
  | --argument-space    | space after argument comma (ld a, b)                              |
  | --no-argument-space | no space after argument comma (ld a,b)                            |
  | --compute-relative  | compute PC relative addresses (jr $1234)                          |
  | --literal-relative  | literal PC relative addresses (jr $+3)                            |
  | --compute-absolute  | compute PC relative addresses (jp $+3)                            |
  | --literal-absolute  | literal PC relative addresses (jp $1234)                          |

Output is to stdout - can redirect to a file in the normal way using

```
ez80-dis --ez80 --start 0x45 --target 0x40000 --address --hex-dump
--compute-relative test.bin

040045  f5              push    af
040046  c5              push    bc
040047  d5              push    de
040048  dd e5           push    ix
04004a  fd e5           push    iy
04004c  ed 73 70 00 04  ld      ($040070),sp
040051  31 ff ff 0a     ld      sp,$0affff
040055  e5              push    hl
040056  cd 7f 00 04     call    $04007f
04005a  e1              pop     hl
04005b  dd 21 08 1f 04  ld      ix,$041f08
040060  dd e5           push    ix
040062  cd 9e 00 04     call    $04009e
040066  06 00           ld      b,$00
040068  c5              push    bc
040069  cd 82 1e 04     call    $041e82
04006d  d1              pop     de
04006e  d1              pop     de
04006f  31 00 00 00     ld      sp,$000000
040073  fd e1           pop     iy
040075  dd e1           pop     ix
040077  d1              pop     de
040078  c1              pop     bc
040079  f1              pop     af
04007a  21 00 00 00     ld      hl,$000000
04007e  c9              ret

```
