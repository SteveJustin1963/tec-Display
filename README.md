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


23.2.2025 ///////////////////////////////////////////////////////////////

I'll analyze the display-related code from the assembly listing and translate it into a MINT function. The key parts are:

1. From the schematic and code:
- Port 1 is used for digit selection (0x01)
- Port 2 is used for segment patterns (0x02)
- Digits are selected by setting bits: 0x20, 0x10, 0x08, 0x04, 0x02, 0x01
- Segment patterns are stored in SEVSEGDATA table starting at 0x80

2. From the display routine (0x02A0):
- It multiplexes 6 digits
- Each digit gets time for persistence 
- The code handles segment patterns and digit selection
- There's delay between digit outputs

Let me create a MINT function for this:

```
// Pattern table for hex digits and letters
// Starting at position 0x80 in original ROM
:P [ #EB #28 #CD #AD #2E #A7 #E7 #29 #EF #2F #6F #E6 #C3 #EC #C7 #47 ] p!

// Digit select patterns
:D [ #20 #10 #08 #04 #02 #01 ] d!

// Display one digit with pattern
// Arguments: digit position (0-5), segment pattern
:S c! b!
// Clear digit select
0 1 /O
// Output segment pattern 
b 2 /O
// Output digit select
d c ? 1 /O
// Delay for persistence
100()
// Clear outputs
0 1 /O
0 2 /O
;

// Convert hex digit to segment pattern
// Takes hex value 0-F, returns segment pattern
:H a!
p a ? b!  // Look up pattern in table p
b         // Return pattern
;

// Display hex digit at position
// Takes: position (0-5), hex value (0-F)
:X c! a!
a H b!   // Convert hex to segments
b c S    // Display segments at position
;

// Example usage:
// 0 #A X  - Shows 'A' in leftmost position
// 5 3 X   - Shows '3' in rightmost position
```

Here's a simplified version without comments that you can enter directly into MINT:

```
:P[#EB#28#CD#AD#2E#A7#E7#29#EF#2F#6F#E6#C3#EC#C7#47]p!
:D[#20#10#08#04#02#01]d!
:S c!b!0 1/O b 2/O d c?1/O 100()0 1/O 0 2/O;
:H a!p a?b!b;
:X c!a!a H b!b c S;
```

To use these functions:

1. First enter the code exactly as shown above
2. Then you can display digits using:
   - Position 0-5 (left to right)
   - Value 0-F (hex)
   - Example: `3 #A X` displays 'A' in position 3
```
100( 3 #A X)

it did something  !




