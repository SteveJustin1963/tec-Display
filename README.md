# tec-Display


## Standard tec1 display
### LED 7-SEGMENT DISPLAY
talking_electronics_11.pdf -p17

with Mint

experiment
```
:A 1 1\> a@ 2\>;
:B a@ + a!;
:C 1000();
:D ABCD;

```
### asm code

```
; Display control register addresses
DISPLAY_CONTROL_1 = 0x00
DISPLAY_CONTROL_2 = 0x01
DISPLAY_DATA = 0x02

; Display control register values
DISPLAY_ENABLE = 0x80
DISPLAY_MODE_GRAPHICS = 0x40
DISPLAY_MODE_TEXT = 0x20
DISPLAY_CURSOR_ENABLE = 0x10
DISPLAY_CURSOR_BLINK = 0x08

; Display data values
CHAR_SPACE = 0x20
CHAR_A = 0x41
CHAR_B = 0x42
CHAR_C = 0x43

; Define a display buffer
DISPLAY_BUFFER: db CHAR_SPACE, CHAR_A, CHAR_B, CHAR_C

; Display control routine
display:
    ; Enable the display
    ld a, DISPLAY_ENABLE
    out (DISPLAY_CONTROL_1), a

    ; Set the display mode to graphics
    ld a, DISPLAY_MODE_GRAPHICS
    out (DISPLAY_CONTROL_1), a

    ; Disable the cursor
    ld a, 0x00
    out (DISPLAY_CONTROL_2), a

    ; Write the display buffer to the display
    ld hl, DISPLAY_BUFFER
    ld a, 0x00
    ld b, 4
    ldir

    ; Return
    ret
```




