# Embedded System Design Lab Programs [BEC601 IPPC]
### Dept. of ECE, KLEIT — 2024-2025

---

## 1. Sum of First 10 Integer Numbers

> Write a program to find the sum of the first 10 integer numbers.

```asm
AREA    ADDITION, CODE, READONLY
        ENTRY                   ; Mark first instruction to execute
        EXPORT __main
__main
        MOV     r0, #10         ; Counter (Integer number 10)
        MOV     r1, #0          ; Partial Sum
        MOV     r2, #1          ; First Number
NEXT    ADD     r1, r1, r2      ; Add sum with the next number
        ADD     r2, #1          ; Increment the next integer
        SUBS    r0, #1          ; Decrement counter
        BNE     NEXT            ; Branch to the loop if not equal to Zero
STOP
        NOP
        END                     ; Mark end of file
```

---

## 2. Multiply Two 16-bit Numbers & Add Two 32-bit Numbers

### i) Multiply Two 16-bit Numbers

> Write an ALP to multiply two 16-bit numbers.

```asm
;/* PROGRAM TO MULTIPLY TWO 16BIT NUMBERS */

;/* VALUE1: 0x1234 (IN R1) */
;/* VALUE2: 0x1234 (IN R2) */
;/* RESULT: 0x14B5A90 (IN R3) */

AREA    MULTIPLY, CODE, READONLY
        ENTRY                   ; Mark first instruction to execute
        EXPORT __main
_main
        LDR     r1, =0x1234     ; Store first number in r1
        LDR     r2, =0x1234     ; Store second number in r2
        MUL     r3, r1, r2      ; Multiplication
L1      B       L1              ; Infinite loop to stop the execution
        NOP
        END                     ; Mark end of file
```

---

### ii) Add Two 32-bit Numbers

> Write an ALP to add two 32-bit numbers.

```asm
;/* PROGRAM TO ADD TWO 32BIT NUMBERS */
;/* VALUE1: 0x12341234 (IN R1) */
;/* VALUE2: 0x12341234 (IN R2) */
;/* RESULT: 0x24682468 (IN R3) */

AREA    ADDITION, CODE, READONLY
        ENTRY                   ; Mark first instruction to execute
        EXPORT main
main
        LDR     r1, =0x12341234 ; Store first number in r1
        LDR     r2, =0x12341234 ; Store second number in r2
        ADD     r3, r1, r2      ; Addition
L1      B       L1              ; Infinite loop to stop the execution
        NOP
        END                     ; Mark end of file
```

---

## 3. Factorial of a Number

> Write a program to find the factorial of a number.

```asm
AREA    FACT, CODE, READONLY
        ENTRY                   ; Mark first instruction to execute
        EXPORT main
main
        MOV     r0, #5          ; Factorial Number
        MOV     r1, #1          ; Result Register
        CMP     r0, #0          ; Compare factorial num with zero
        BEQ     STOP            ; If zero stop
        MOV     r1, r0          ; Copy the number
NEXT    SUBS    r0, #1          ; Decrement the number
        CMP     r0, #0          ; Compare it with zero
        BEQ     STOP            ; If zero stop
        MUL     r2, r1, r0      ; Multiply with the next number
        MOV     r1, r2          ; Save the result
        B       NEXT            ; Branch to the loop
STOP
        NOP
        END                     ; Mark end of file
```

---

## 4. Add an Array of 16-bit Numbers (32-bit Result)

> Write a program to add an array of 16-bit numbers and store the 32-bit result in internal RAM.

```asm
AREA    ADDITION, CODE, READONLY
        ENTRY                   ; Mark first instruction to execute
        EXPORT __main

__main
        LDR     R0, =ARRAY
        MOV     R1, #0
        MOV     R4, #0
NEXT    CMP     R1, #6
        BEQ     STORE
        LDRH    R3, [R0], #2
        ADC     R4, R4, R3
        ADD     R1, R1, #1
        B       NEXT
STORE   LDR     R5, =SUM
        STR     R4, [R5]
STOP    B       STOP

ARRAY   DCW     0x0001, 0x0002, 0x0003, 0x0004, 0x0005, 0x0006

        AREA    RESULT, DATA, READWRITE
SUM     DCD     0
        END
```

---

## 5. Square of a Number (1 to 10) using Look-up Table

> Write a program to find the square of a number (1 to 10) using a look-up table.

```asm
AREA    SQUARE, CODE, READONLY
        ENTRY                           ; Mark first instruction to execute
        EXPORT __main
__main
        LDR     R0, = TABLE1            ; Load start address of Lookup table
        LDR     R1, = 6                 ; Load number whose square is to be found
        MOV     R1, R1, LSL#0x2         ; Generate address corresponding to square of given no
        ADD     R0, R0, R1              ; Load address of element in Lookup table
        LDR     R3, [R0]                ; Get square of given no in R3
        NOP                             ; Lookup table contains Squares of no's from 0 to 10

TABLE1  DCD     0X00000000              ; SQUARE OF 0  = 0
        DCD     0X00000001              ; SQUARE OF 1  = 1
        DCD     0X00000004              ; SQUARE OF 2  = 4
        DCD     0X00000009              ; SQUARE OF 3  = 9
        DCD     0X00000010              ; SQUARE OF 4  = 16
        DCD     0X00000019              ; SQUARE OF 5  = 25
        DCD     0X00000024              ; SQUARE OF 6  = 36
        DCD     0X00000031              ; SQUARE OF 7  = 49
        DCD     0X00000040              ; SQUARE OF 8  = 64
        DCD     0X00000051              ; SQUARE OF 9  = 81
        DCD     0X00000064              ; SQUARE OF 10 = 100
        END                             ; Mark end of file
```

---

## 6. Find the Largest/Smallest Number in an Array of 32-bit Numbers (N=7)

> Write a program to find the largest/smallest number in an array of 32-bit numbers (N=7).

```asm
AREA    LARGEST, CODE, READONLY
        ENTRY                   ; Mark first instruction to execute
        EXPORT __main
__main
        MOV     R5, #6          ; Initialise counter to 6 (i.e. N=7)
        LDR     R1, =VALUE1     ; Load the address of first value
        LDR     R2, [R1], #4    ; Word align to array element

LOOP    LDR     R4, [R1], #4    ; Word align to array element
        CMP     R2, R4          ; Compare numbers
        BHI     LOOP1           ; If the first number is > then goto LOOP1
        MOV     R2, R4          ; If the first number is < then move content R4 to R2

LOOP1   SUBS    R5, R5, #1      ; Decrement counter
        CMP     R5, #0          ; Compare counter to 0
        BNE     LOOP            ; Loop back till array ends
        LDR     R4, =RESULT     ; Load the address of result
        STR     R2, [R4]        ; Store the result
        NOP
        NOP
        NOP

VALUE1                          ; Array of 32-bit numbers (N=7)
        DCD     0X44444444
        DCD     0X22222222
        DCD     0X11111111
        DCD     0X33333333
        DCD     0XAAAAAAAA
        DCD     0X88888888
        DCD     0X99999999

        AREA    DATA2, DATA, READWRITE  ; To store result in given address
RESULT  DCD     0X0
        END                     ; Mark end of file
```

---

## 7. Arrange 32-bit Numbers in Ascending/Descending Order

> Write a program to arrange a series of 32-bit numbers in ascending/descending order.

```asm
AREA    ASCENDING, CODE, READONLY
        ENTRY                   ; Mark first instruction to execute
        EXPORT __main
__main
        MOV     R8, #4          ; Initialise counter to 4 (i.e. N=4)
        LDR     R2, =CVALUE     ; Address of code region
        LDR     R3, =DVALUE     ; Address of data region
LOOP0   LDR     R1, [R2], #4    ; Loading values from code region
        STR     R1, [R3], #4    ; Storing values to data region
        SUBS    R8, R8, #1      ; Decrement counter
        CMP     R8, #0          ; Compare counter to 0
        BNE     LOOP0           ; Loop back till array ends

START1  MOV     R5, #3          ; Initialise counter to 3 (i.e. N=4)
        MOV     R7, #0          ; Flag to denote exchange has occurred
        LDR     R1, =DVALUE     ; Load the address of first value
LOOP1   LDR     R2, [R1], #4    ; Word align to array element
        LDR     R3, [R1]        ; Load second number
        CMP     R2, R3          ; Compare numbers
        BLT     LOOP2           ; If the first number is < then goto LOOP2
        STR     R2, [R1], #-4   ; Interchange number R2 & R3
        STR     R3, [R1]        ; Interchange number R2 & R3
        MOV     R7, #1          ; Flag denoting exchange has taken place
        ADD     R1, #4          ; Restore the pointer
LOOP2   SUBS    R5, R5, #1      ; Decrement counter
        CMP     R5, #0          ; Compare counter to 0
        BNE     LOOP1           ; Loop back till array ends
        CMP     R7, #0          ; Comparing flag
        BNE     START1          ; If flag is not zero then go to START1
        NOP
        NOP
        NOP

CVALUE                          ; Array of 32-bit numbers (N=4) in Code Region
        DCD     0X44444444
        DCD     0X11111111
        DCD     0X33333333
        DCD     0X22222222

        AREA    DATA1, DATA, READWRITE  ; Array of 32-bit numbers in Data Region
DVALUE  DCD     0X00000000
        END                     ; Mark end of file
```

---

## 8. Count the Number of Ones and Zeros in Two Consecutive Memory Locations

> Write a program to count the number of ones and zeros in two consecutive memory locations.

```asm
AREA    SEARCH1, CODE, READONLY
        ENTRY                   ; Mark first instruction to execute
        EXPORT __main
__main
        MOV     r5, #2
        MOV     r0, #0
        MOV     r1, #0
        LDR     r2, VALUE1
SECOND  MOV     r3, #32
NEXT    MOVS    r2, r2, ror #1
        BLO     ZERO
        ADD     r1, #1
        B       LAST
ZERO    ADD     r0, #1
LAST    SUBS    r3, #1
        BNE     NEXT            ; Branch to the loop if not equal to Zero
        LDR     r2, VALUE2
        SUBS    r5, #1
        BNE     SECOND
STOP    B       STOP
        NOP

VALUE1  DCD     0X1234567A      ; Given first number in memory
VALUE2  DCD     0X1234567A      ; Given second number in memory
        END                     ; Mark end of file
```

---

## 9. Stepper Motor Interface — Clockwise and Anti-clockwise Rotation

> Interface a Stepper motor and rotate it in clockwise and anti-clockwise direction.

```c
#include <LPC17xx.H>

void clock_wise(void);
void anti_clock_wise(void);

unsigned long int var1, var2;
unsigned int i = 0, j = 0, k = 0;

int main(void)
{
    LPC_PINCON->PINSEL4 = 0x00000000;  // P2.0 to P2.3 GPIO
    LPC_GPIO2->FIODIR   = 0x0000000F;  // P2.0 to P2.3 output

    while (1)
    {
        for (j = 0; j < 50; j++)        // 50 times Clockwise Rotation
            clock_wise();

        for (k = 0; k < 65000; k++);    // Delay to show Anti-clock Rotation

        for (j = 0; j < 50; j++)        // 50 times Anti-Clockwise Rotation
            anti_clock_wise();

        for (k = 0; k < 65000; k++);    // Delay to show Clock Rotation
    }                                   // End of while(1)
}                                       // End of main

void clock_wise(void)
{
    var1 = 0x00000001;                  // For Clockwise
    for (i = 0; i <= 3; i++)            // For A B C D Stepping
    {
        LPC_GPIO2->FIOCLR = 0X0000000F;
        LPC_GPIO2->FIOSET = var1;
        var1 = var1 << 1;               // For Clockwise
        for (k = 0; k < 15000; k++);   // For step speed variation
    }
}

void anti_clock_wise(void)
{
    var1 = 0x00000008;                  // For Anticlockwise
    for (i = 0; i <= 3; i++)            // For A B C D Stepping
    {
        LPC_GPIO2->FIOCLR = 0X0000000F;
        LPC_GPIO2->FIOSET = var1;
        var1 = var1 >> 1;               // For Anticlockwise
        for (k = 0; k < 15000; k++);   // For step speed variation
    }
}
```

---

## 10. DAC Interface — Triangular and Square Waveform Generation

> Interface a DAC and generate Triangular and Square waveforms.

### Square Wave

```c
#include "LPC17xx.h"

void delay(unsigned int ms)             // Delay subroutine
{
    unsigned int i, j;
    for (i = 0; i < ms; i++)
        for (j = 0; j < 2000; j++);
}

int main()
{
    SystemInit();
    LPC_PINCON->PINSEL1 |= 0x02 << 20; // P0.26 PINSEL bits 20 and 21

    while (1)
    {
        LPC_DAC->DACR = 0xffff;         // Send maximum value to DAC
        delay(20);                      // Delay
        LPC_DAC->DACR = 0x0000;         // Send minimum value to DAC
        delay(20);
    }
}
```

---

### Triangular Wave

```c
#include "LPC17xx.h"

uint32_t val;

int main()
{
    SystemInit();
    LPC_PINCON->PINSEL1 |= 0x02 << 20; // P0.26 PINSEL bits 20 and 21

    while (1)
    {
        while (1)
        {
            val++;                      // Increment value
            LPC_DAC->DACR = (val << 6); // Send value to DAC
            if (val >= 0x3ff)           // If value exceeds 1024
            {
                break;
            }
        }

        while (1)
        {
            val--;                      // Decrement value
            LPC_DAC->DACR = (val << 6); // Send value to DAC
            if (val <= 0x000)           // If value comes down to 0
            {
                break;
            }
        }
    }
}
```

---

## 11. Display Hex Digits 0 to F on a 7-Segment LED Interface

> Display the Hex digits 0 to F on a 7-segment LED interface, with a suitable delay in between.

```c
/*
 * Description: DISPLAYS ARE CONNECTED IN COMMON CATHODE MODE
 * Port0 Connected to data lines of all 7-segment displays
 *
 *      a
 *    ----
 *  f| g  |b
 *   |----|
 *  e|    |c
 *    ----  . dot
 *      d
 *
 *  a   = P0.04
 *  b   = P0.05
 *  c   = P0.06
 *  d   = P0.07
 *  e   = P0.08
 *  f   = P0.09
 *  g   = P0.10
 *  dot = P0.11
 */

#include <LPC17xx.h>

unsigned int delay, count = 0, Switchcount = 0, j;

unsigned int Disp[16] = {
    0x000003f0,  // 0
    0x00000060,  // 1
    0x000005b0,  // 2
    0x000004f0,  // 3
    0x00000660,  // 4
    0x000006d0,  // 5
    0x000007d0,  // 6
    0x00000070,  // 7
    0x000007f0,  // 8
    0x000006f0,  // 9
    0x00000770,  // A
    0x000007c0,  // B
    0x00000390,  // C
    0x000005e0,  // D
    0x00000790,  // E
    0x00000710   // F
};

#define ALLDISP   0x00180000   // Select all display
#define DATAPORT  0x00000ff0   // P0.4 to P0.11: Data lines for Seven Segments

int main(void)
{
    LPC_PINCON->PINSEL0 = 0x00000000;
    LPC_PINCON->PINSEL1 = 0x00000000;
    LPC_GPIO0->FIODIR   = 0x00180ff0;

    while (1)
    {
        LPC_GPIO0->FIOSET |= ALLDISP;
        LPC_GPIO0->FIOCLR  = 0x00000ff0;       // Clear data lines to 7-segment displays
        LPC_GPIO0->FIOSET  = Disp[Switchcount]; // Get 7-segment display value from array

        for (j = 0; j < 3; j++)
            for (delay = 0; delay < 30000; delay++); // Delay

        Switchcount++;
        if (Switchcount == 0x10)                // 0 to F has been displayed? Go back to 0
        {
            Switchcount = 0;
            LPC_GPIO0->FIOCLR = 0x00180ff0;
        }
    }
}
```

---

## 12. Switch Interface — Status Display via Relay, Buzzer and LED

> Interface a simple Switch and display its status through Relay, Buzzer and LED.

```c
#include <LPC17xx.H>

int main(void)
{
    LPC_PINCON->PINSEL1 = 0x00000000;  // P0.24, P0.25 GPIO
    LPC_GPIO0->FIODIR   = 0x03000000;  // P0.24 output for Buzzer
                                       // P0.25 output for Relay/LED

    while (1)
    {
        if (!(LPC_GPIO2->FIOPIN & 0x00000800))  // Is GP_SW (SW4) pressed?
        {
            LPC_GPIO0->FIOSET = 0x03000000;     // Relay ON
        }
        else
        {
            LPC_GPIO0->FIOCLR = 0x03000000;     // Relay OFF
        }
    }
}                                               // End of main
```
