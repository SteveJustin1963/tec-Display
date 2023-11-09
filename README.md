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







## Iterate
- Graphics displays, segment and matrix
- 8x8
- VDU (eg EA1978Feb)
- [OLED display](https://github.com/SteveJustin1963/tec-OLED)
- pixxiLCD-13P2-CTP-CLB TFT LCD Colour Display https://4dsystems.com.au/pixxilcd-13p2-ctp-clb
- VFD display https://en.wikipedia.org/wiki/Vacuum_fluorescent_display
- NE2 7-SEGMENT DISPLAY
- [Vectrex](https://github.com/SteveJustin1963/tec-Vectrex)
- [Spin POV](https://github.com/SteveJustin1963/tec-POV)
- [Laser Raster mag-mech-xy](https://github.com/SteveJustin1963/tec-Laser-Raster-Display)
- [Flip Clock](https://github.com/SteveJustin1963/tec-Flip-Clock)
- Incandescent 7-SEGMENT DISPLAY https://hackaday.com/2018/11/06/soviet-era-7-segment-display-built-like-a-tank/
- Nixie tube https://en.wikipedia.org/wiki/Nixie_tube
- [CNC pen plotter](https://github.com/SteveJustin1963/tec-CNC-PEN)
- Electric Typewriter
- [Etch a Sketch](https://github.com/SteveJustin1963/tec-Etch-A-Sketch)
- Tyco Doodle Dome https://makezine.com/2006/12/21/digitized-doodle-dome/  https://picclick.com/Vintage-Tyco-Doodle-Dome-172621314741.html
- CRT Display
- https://www.industrialalchemy.org/tubepage.php?item=10&user=0
- https://github.com/SteveJustin1963/tec-OLD
- 


## Ref
- https://randomnerdtutorials.com/guide-for-oled-display-with-arduino/
- https://en.wikipedia.org/wiki/Etch_A_Sketch
- 
