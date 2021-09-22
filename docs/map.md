#

## Memory map

Atari XE/XL

The compiler uses the zero page in the range $0080 .. $00D7

When using additional memory, the bank code array for `PORTB` is placed from $0101 ... $0140

An additional 256 byte memory buffer used for string operations is located from $0400 ... $04FF
