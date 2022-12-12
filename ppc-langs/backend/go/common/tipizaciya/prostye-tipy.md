# Простые типы

```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr
     // (причем нельзя сложить int16 и int32 без явного преобразования)

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point
     // Любую строку можно перевести в []rune

float32 float64

complex64 complex128
```
