#

## [Comments](http://www.freepascal.org/docs-html/ref/refse2.html)

In **MP** `//` is used to mark a one-line comment and `{ }` or `(* *)` mark a multiline comment.

```delphi
// this is a comment
inc(a); // this also is a comment

(* comment *)

(*

  comment

*)

{ this
  is
  a comment
}
```

## Reserved identifiers

### reserved commands

```delphi
absolute           and                 array              asm               assembler
begin              case                const              constructor       div
do                 downto              else               end               exports
external           file                for                forward           function
if                 implementation      in                 interrupt         interface
library            main                mod                not               object
of                 or                  overload           pascal            procedure
program            record              register           repeat            shl
shr                string              text               textfile          then
to                 type                unit               until             uses
var                while               xor
```

### reserved constants

```delphi
pi
true
false
nil
eol
nan
infinity
neginfinity
```

## Expressions

### [Numbers](http://www.freepascal.org/docs-html/ref/refse6.html)

#### decimal notation
```delphi
-100
-2437325
1743
```
#### hexadecimal notation
```delphi
$100
$e430
$000001
```
#### binary notation
```delphi
%0001001010
%000000001
%001000
```
#### ATASCII code notation
```delphi
'a'
'fds'
'W'
#65#32#65
#$9b
```

### Operators

#### arithmetic

```delphi
+   Addition
-   Subtraction
*   Multiplication
/   Division
DIV Integer division
MOD Remainder
```

#### bitwise

```delphi
NOT Bitwise negation (unary)
AND Bitwise and
OR  Bitwise or
XOR Bitwise xor
SHL Bitwise shift to the left
SHR Bitwise shift to the right
```

#### logical

```delphi
NOT logical negation (unary)
AND logical and
OR  logical or
XOR logical xor
```

#### relational

```delphi
=   Equal
<>  Not equal
<   Less than
>   Greater than
<=  Less than or equal
>=  Greater than or equal
```

## [Compiler directives](http://www.freepascal.org/docs-html/prog/progch1.html#x5-40001)

Compiler directives are of form::

```delphi
{$directive parameters}
{$list_of_switch_directives}
```

A directive is a comment differentiated from a regular comment by the first $ character.

### [CONDITIONAL](https://wiki.freepascal.org/Conditional_compilation)

```delphi
{$IFDEF label}
{$IFNDEF label}
{$ELSE}
{$ENDIF}
{$DEFINE label}
{$UNDEF label}
```

```delphi
{$define test}

const
  {$ifdef test}
  a=1;
  {$else}
  a=2;
  {$endif}
```
From the assembly level access to defined $DEFINE directives is only possible through `MAIN.@DEFINES.label`.


### [$CODEALIGN PROC = value](https://www.freepascal.org/docs-html/prog/progsu9.html)

The `$CODEALIGN PROC` directive allows the generated result code to be aligned to the `VALUE` bytes of the memory page. A `.ALIGN VALUE` code is inserted before each `PROCEDURE`, `FUNCTION` block. To disable alignment, set `{$CODEALIGN PROC = 0}`.

### [$CODEALIGN LOOP = value](https://www.freepascal.org/docs-html/prog/progsu9.html)

The `$CODEALIGN LOOP` directive allows the generated result code to be aligned to the `VALUE` bytes of the memory page. A `.ALIGN VALUE` code is inserted before each `FOR`, `WHILE`, `REPEAT` iteration instruction. To disable alignment, set `{$CODEALIGN LOOP = 0}`.


### [$DEFINE](https://www.freepascal.org/docs-html/prog/progsu11.html#x18-170001.2.11)

#### `BASICOFF`

```delphi
{$DEFINE BASICOFF}
```

Additional code block that shutdown **ATARI BASIC**.

#### `ROMOFF`

```delphi
{$DEFINE ROMOFF}
```

We gain access to the memory *under the ROM*: `$C000..$CFFF`, `$D800..$FFFF`.

The character set from **ROM** `$E000..$E3FF` is rewritten to the same address in **RAM**, the interrupt handler `NMI`, `IRQ` is installed. The operating system works normally, you can call the procedures contained in it from the **ASM** using the macro `m@call`.

> **WARNING:**  
> _When the **ANTIC Display List** program is placed under the **ROM**, each key press will trigger an **IRQ** interrupt that handles the keyboard._
>
> _The **ANTIC** program will be interfered with by **ROM** - **RAM** switching, in case we use the **Display List** interrupt (**DLI**) the stack may be damaged and the system may crash._

#### `NOROMFONT`
```
{$DEFINE NOROMFONT}
```
Supplement for `{$DEFINE ROMOFF}`, prevents rewriting of character set from ROM to RAM


### [$ERROR](https://www.freepascal.org/docs-html/prog/progsu17.html#x24-230001.2.17)

```delphi
{$ERROR user_defined}
```
Generate error message.


### [$F, $FASTMUL](https://codebase64.org/doku.php?id=base:seriously_fast_multiplication)

```delphi
{$fastmul page}   // fastmul at page*256
{$f $70}          // fastmul at $7000
```
Alternative procedures for fast multiplication of the `BYTE` `SHORTINT` `WORD` `SMALLINT` `SHORTREAL` types. The procedures occupy **2KB** and are located starting from the address __PAGE*256__.


### [$I+, $I-, IOCHECK](https://www.freepascal.org/docs-html/prog/progsu38.html#x45-440001.2.38)

```delphi
{$I+}
{$I-}
```

    {i+}  IOCHECK ON  default
    {i-}  IOCHECK OFF


For `{$i+}` in case of **I/O** transmission errors `RESET` `REWRITE` `BLOCKREAD` `BLOCKWRITE` `CLOSE` the ran program is stopped and an error diagnostic `ERROR xxx` is generated. Disabling `IOCHECK {$i-}` is of use for file existence checking, for example:

```delphi
function FileExists(name: TString): Boolean;
var f: file;
begin

  {$I-}     // io check off
  Assign (f, name);
  Reset (f);
  Result:=(IoResult<128) and (length(name)>0);
  Close (f);
  {$I+}     // io check on

end;
```

In `PROCEDURE` and `FUNCTION` blocks, the `IOCHECK` directive is of local scope, after finishing the compilation of such block the previous value of `IOCHECK`, defined outside of such block, is restored.

### [$I, $INCLUDE](https://www.freepascal.org/docs-html/prog/progsu41.html)

#### `%DATE%`

```delphi
{$INCLUDE %DATE%}
{$I %DATE%}
```
Directive parameter `%DATE%` for inclusion of a string with current compilation date.

#### `%TIME%`

```delphi
{$INCLUDE %TIME%}
{$I %TIME%}
```
Directive parameter `%TIME%` for inclusion of a string with current compilation time.

#### `FILENAME`

```delphi
{$INCLUDE filename}
{$I filename}
```
Directive parameter `FILENAME` to attach the text contained in the file.


### [$INFO](https://www.freepascal.org/docs-html/prog/progsu35.html#x42-410001.2.35)

```delphi
{$INFO user_defined}
```
Generate info message.


### [$LIBRARYPATH](https://www.freepascal.org/docs-html/prog/progsu99.html)

```delphi
{$LIBRARYPATH path1;path2;...}
```
Directive to indicate additional search paths for libraries `unit`.

### [$MACRO](https://www.freepascal.org/docs-html/prog/progse5.html)

```delphi
{$MACRO ON}
{$MACRO OFF}
{$MACRO+}
{$MACRO-}
```
The `{$macro }` directive enables/disables the ability to [define macros](../macros/#define-macros), is required by **FPC**, in **MP** it is retained for compatibility purposes only.


### [$OPTIMIZATION](https://www.freepascal.org/docs-html/prog/progsu58.html)

#### `LOOPUNROLL`

```delphi
{$OPTIMIZATION loopunroll}
```
The `$OPTIMIZATION` directive with the `LOOPUNROLL` parameter allows you to unroll `FOR` loops (the parameters of such a loop must be constants):
```
{$OPTIMIZATION loopunroll}

 for i:=0 to 11 do tab[i]:=i*2;
 
{$OPTIMIZATION noloopunroll}
```

#### `NOLOOPUNROLL`

```delphi
{$OPTIMIZATION noloopunroll}
```
The `NOLOOPUNROLL` parameter disables the `FOR` loop unroll.


### [$R, $RESOURCE](https://www.freepascal.org/docs-html/prog/progsu67.html#x74-730001.2.67)

```delphi
{$R filename}
{$RESOURCE filename}

RCLABEL RCTYPE RCFILE [PAR0 PAR1 PAR2 PAR3 PAR4 PAR5 PAR6 PAR7]
```

Directive to attach a resource file. A resource file is a text file, each of its successive lines should consist of three fields separated by a *white character*: `RCLABEL`, the label (its declaration must also be included in the program), `RCTYPE`, the resource type and `RCFILE`, the file location. Currently, the `BASE\RES6502.ASM` file contains macros to support 10 types of `RCTYPE` resources:

#### `RCDATA`

Any data type.

#### `EXTMEM`

Any data type loaded into **PORTB** secondary memory, loading address determined by `RCLABEL`.  

#### `RCASM`

An assembly file that will be attached and assembled.

#### `DOSFILE`

**Atari DOS** header file, the loading address of such a file should be the same as `RCLABEL`.

#### `RELOC`

A relocatable file in **MadAssembler** format, the file will be relocated to the address indicated by `RCLABEL`.

#### `RMT`

*Raster Music Tracker* module file, the file will be relocated to the address indicated by `RCLABEL`.

#### `MPT`

*The Music ProTracker* module file, the file will be relocated to the indicated address `RCLABEL`.

#### `CMC`

**Chaos Music Composer** module file, the file will be relocated to the address indicated by `RCLABEL`.

#### `RMTPLAY`

Player for the **RMT** module, with the `*.FEAT` file passed as the `RCFILE` and the player mode `0..3` passed as the `PAR0`.

```none
    0 => compile RMTplayer for 4 tracks mono
    1 => compile RMTplayer for 8 tracks stereo
    2 => compile RMTplayer for 4 tracks stereo L1 R2 R3 L4
    3 => compile RMTplayer for 4 tracks stereo L1 L2 R3 R4
```

#### `MPTPLAY`

Player for **MPT** module.

#### `CMCPLAY`

Player for **CMC** module.

#### `XBMP`

**Windows Bitmap** file (8 BitsPerPixel) loaded into **VBXE** memory to the indicated address `RCLABEL` from `PAR0` color index in **VBXE** color palette no. 1.

Example:

```none
bmp1  RCDATA   'pic.mic'
msx   MPT      'porazka.mpt'
play  RMTPLAY  'modul.feat' 1
bmp   XBMP     'pic.bmp' 80
```


### [$WARNING](https://www.freepascal.org/docs-html/prog/progsu81.html#x88-870001.2.81)

```delphi
{$WARNING user_defined}
```
Generate warning message.


