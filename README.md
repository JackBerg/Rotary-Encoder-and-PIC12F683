# Rotary-Encoder-and-PIC12F683
Here's the PBP3 Pro Basic Compiler code 
  Simple Rotary Encoder program using PIC12f683 with PBP3 Pro Basic Compiler
  it use the rise-edge of GPIO3 and latch the direction
  no need for bounce capacitor timing

'----------------------------------------------------------------------------
'  Name    : Rotary Encoder PIC12F683 NORMAL & REVERSE + SW  
'  Date    : Aug 22-2024                                             
'  Note    : GPIO.1,2,3=LED or Output INPUT A,B = GPIO.4,5
'  Memory  : 157 words used                                           
'  PBP3    : PBASIC PRO COMPILER
'----------------------------------------------------------------------------
#CONFIG             
  __config _INTOSCIO & _WDT_OFF & _PWRTE_OFF & _MCLRE_OFF & _CP_ON & _CPD_OFF & _BOD_OFF & _IESO_OFF & _FCMEN_OFF
#ENDCONFIG
DEFINE OSC 8
ANSELA = 0                  ' Set all digital
CMCON = 7                   ' Analog comparators off
GPIO = %000000              ' *** INITIALIZE GPIO TO ZERO BEFORE TRISIO
TRISIO = %111000            ' 1=RA3,4,5 INPUT / 0=RA0,1,2 OUTPUT
OSCCON = $70                ' $70=8Mhz  $60=4mhz  $50=2MHZ  $40=1MHZ  $30=500KHZ

DIR VAR BYTE
Latch VAR BYTE

do
Latch = 0 

' Rotary SW detection 
IF GPIO.3 = 1 THEN ;Rotary SW detected
    do while GPIO.3 = 1
    GPIO.0 = 1
    loop
    GPIO.0 = 0
endif

' Direction detection
IF GPIO.4 = 1 AND GPIO.5 = 0 THEN ;DIR=1 
        DIR = 1  
endif

' Direction detection
IF GPIO.4 = 0 AND GPIO.5 = 1 THEN ;DIR=0 
        DIR = 0   
endif

' Send output to Led
IF GPIO.4 = 1 AND GPIO.5 = 1 and Latch = 0 THEN 
        Latch = 1
          IF DIR = 0 then
            GPIO.2 = 1 
          ELSE
            GPIO.1 = 1 
          endif
       pause 5      ; 5ms Pulse
       GPIO=0
endif
LOOP
