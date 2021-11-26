#include "p16F628a.inc"    
 __CONFIG _FOSC_INTOSCCLK & _WDTE_OFF & _PWRTE_OFF & _MCLRE_ON & _BOREN_OFF & _LVP_OFF & _CPD_OFF & _CP_OFF    

RES_VECT  CODE    0x0000            ; processor reset vector
    GOTO    START                   ; go to beginning of program
; TODO ADD INTERRUPTS HERE IF USED

INT_VECT CODE 0x004 ; interrupt vector
	;ISR code
 
	DECFSZ CNT
	GOTO $+7
	CALL melody
	INCF note
	MOVLW D'4' ; 50mS * value
	MOVWF CNT
	    ; 256uS * 195 =~ 50mS
	    ; 255 - 195 = 60
	MOVLW D'100' ; preload value
	MOVWF TMR0
	bcf INTCON, T0IF ; clr TMR0 interrupt flag
	retfie ; return from interrupt
    
MAIN_PROG CODE                      ; let linker place main program

ij EQU 0x20
ji EQU 0x21
CNT equ 0x22
note equ 0x23
x equ 0x24
y equ 0x25
p equ 0x26
a equ 0x27
i equ 0x28
j equ 0x29
k equ 0x30
ii equ 0x31
jj equ 0x32
kk equ 0x33
f equ 0x34

START
    
    
    MOVLW 0x07
    MOVWF CMCON
    BCF STATUS, RP1
    BSF STATUS, RP0 
    CLRF TRISB
    CLRF TRISA
    MOVLW 0x00
    MOVWF TRISA
    MOVWF TRISB
    MOVLW b'10000111'
    MOVWF OPTION_REG
    
    BCF STATUS, RP0
    
   
    
			; setup TMR0 INT
    bsf INTCON, GIE	; enable global interrupt
    bsf INTCON, T0IE	; enable TMR0 interrupt
    bcf INTCON, T0IF	; clr TMR0 interrupt flag to turn on,
			; must be cleared after interrupt
			; 256uS * 195 =~ 50mS
			; 255 - 195 = 60
    MOVLW D'60'		; preload value
    MOVWF TMR0
    MOVLW D'10'		; 50mS * 20 = 1 Sec.
    MOVWF CNT
    CLRF PORTA
    CLRF PORTB
    CLRF note
    
    
INITLCD
    BCF PORTA,0		;reset
    MOVLW 0x01
    MOVWF PORTB
    

    BSF PORTA,1		;exec
    CALL time
    BCF PORTA,1
    CALL time
    
    MOVLW 0x0C		;first line
    MOVWF PORTB
    
    BSF PORTA,1		;exec
    CALL time
    BCF PORTA,1
    CALL time
         
    MOVLW 0x3C		;cursor mode
    MOVWF PORTB
    
    BSF PORTA,1		;exec
    CALL time
    BCF PORTA,1
    CALL time
    
    CALL TITULO  

    
INICIO:
    
    bcf PORTA,2
    CALL tiempos
    nop
    nop
    nop
    nop
    bsf PORTA,2
    call tiempos    
    nop
    nop
    
    GOTO INICIO
   
;rutina de tiempo
    tiempos:
	    movfw i
	    movwf ii
    iloop:  movfw j 
	    movwf jj
    jloop:  decfsz jj,f
	    goto jloop
	    decfsz ii,f
	    goto iloop
	    movfw k 
	    movwf kk
	    decfsz kk
	    goto $-1
	    return
	       
	    
    melody:
        MOVFW note
    XORLW d'197'
    BTFSS STATUS,Z
    GOTO $+2
    CLRF note
	;---------------------------------------------------------------------------------
   ;si3
    MOVFW note
    XORLW d'0'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'12'
    MOVWF i
    MOVLW D'25'
    MOVWF j
    MOVLW D'18'
    MOVWF k
    RETURN
       ;si3
    MOVFW note
    XORLW d'1'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'12'
    MOVWF i
    MOVLW D'25'
    MOVWF j
    MOVLW D'18'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'2'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'3'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;do4
    MOVFW note
    XORLW d'4'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'11'
    MOVWF i
    MOVLW D'26'
    MOVWF j
    MOVLW D'14'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'5'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;re4
    MOVFW note
    XORLW d'6'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'11'
    MOVWF i
    MOVLW D'23'
    MOVWF j
    MOVLW D'12'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'7'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;re4
    MOVFW note
    XORLW d'8'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'11'
    MOVWF i
    MOVLW D'23'
    MOVWF j
    MOVLW D'12'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'9'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;do4
    MOVFW note
    XORLW d'10'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'11'
    MOVWF i
    MOVLW D'26'
    MOVWF j
    MOVLW D'14'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'11'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;si3
    MOVFW note
    XORLW d'12'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'12'
    MOVWF i
    MOVLW D'25'
    MOVWF j
    MOVLW D'18'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'13'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;la3
    MOVFW note
    XORLW d'14'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'13'
    MOVWF i
    MOVLW D'27'
    MOVWF j
    MOVLW D'7'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'15'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;sol3
    MOVFW note
    XORLW d'16'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'14'
    MOVWF i
    MOVLW D'28'
    MOVWF j
    MOVLW D'11'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'17'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;sol3
    MOVFW note
    XORLW d'18'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'14'
    MOVWF i
    MOVLW D'28'
    MOVWF j
    MOVLW D'11'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'19'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;la3
    MOVFW note
    XORLW d'20'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'13'
    MOVWF i
    MOVLW D'27'
    MOVWF j
    MOVLW D'7'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'21'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;si3
    MOVFW note
    XORLW d'22'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'12'
    MOVWF i
    MOVLW D'25'
    MOVWF j
    MOVLW D'18'
    MOVWF k
    RETURN
	
;esp
    MOVFW note
    XORLW d'23'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;si3
    MOVFW note
    XORLW d'24'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'12'
    MOVWF i
    MOVLW D'25'
    MOVWF j
    MOVLW D'18'
    MOVWF k
    RETURN
;esp -------------------para el si la la
    MOVFW note
    XORLW d'25'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
    ;esp
    MOVFW note
    XORLW d'26'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;la3
    MOVFW note
    XORLW d'27'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'13'
    MOVWF i
    MOVLW D'27'
    MOVWF j
    MOVLW D'7'
    MOVWF k
    RETURN
    ;la3
    MOVFW note
    XORLW d'28'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'13'
    MOVWF i
    MOVLW D'27'
    MOVWF j
    MOVLW D'7'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'29'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
    ;esp
    MOVFW note
    XORLW d'30'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
    ;-------------------segunda---------------------------
;si3
    MOVFW note
    XORLW d'31'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'12'
    MOVWF i
    MOVLW D'25'
    MOVWF j
    MOVLW D'18'
    MOVWF k
    RETURN
    ;si3
    MOVFW note
    XORLW d'32'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'12'
    MOVWF i
    MOVLW D'25'
    MOVWF j
    MOVLW D'18'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'33'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'34'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;do4
    MOVFW note
    XORLW d'35'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'11'
    MOVWF i
    MOVLW D'26'
    MOVWF j
    MOVLW D'14'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'36'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;re4
    MOVFW note
    XORLW d'37'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'11'
    MOVWF i
    MOVLW D'23'
    MOVWF j
    MOVLW D'12'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'38'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;re4
    MOVFW note
    XORLW d'39'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'11'
    MOVWF i
    MOVLW D'23'
    MOVWF j
    MOVLW D'12'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'40'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;do4
    MOVFW note
    XORLW d'41'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'11'
    MOVWF i
    MOVLW D'26'
    MOVWF j
    MOVLW D'14'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'42'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;si3
    MOVFW note
    XORLW d'43'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'12'
    MOVWF i
    MOVLW D'25'
    MOVWF j
    MOVLW D'18'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'44'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW D'0'
    MOVWF i
    MOVLW D'0'
    MOVWF j
    MOVLW D'0'
    MOVWF k
    RETURN
;la3
    MOVFW note
    XORLW d'45'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'13'
    MOVWF i
    MOVLW d'27'
    MOVWF j
    MOVLW d'7'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'46'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;sol3
    MOVFW note
    XORLW d'47'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'14'
    MOVWF i
    MOVLW d'28'
    MOVWF j
    MOVLW d'11'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'48'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;sol3
    MOVFW note
    XORLW d'49'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'14'
    MOVWF i
    MOVLW d'28'
    MOVWF j
    MOVLW d'11'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'50'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;la3
    MOVFW note
    XORLW d'51'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'13'
    MOVWF i
    MOVLW d'27'
    MOVWF j
    MOVLW d'7'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'52'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;si3
    MOVFW note
    XORLW d'53'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'12'
    MOVWF i
    MOVLW d'25'
    MOVWF j
    MOVLW d'18'
    MOVWF k
    RETURN
	
;esp
    MOVFW note
    XORLW d'54'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN    
;la3
    MOVFW note
    XORLW d'55'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'13'
    MOVWF i
    MOVLW d'27'
    MOVWF j
    MOVLW d'7'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'56'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN  
    ;esp
    MOVFW note
    XORLW d'57'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;sol3
    MOVFW note
    XORLW d'58'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'14'
    MOVWF i
    MOVLW d'28'
    MOVWF j
    MOVLW d'11'
    MOVWF k
    RETURN
;sol3
    MOVFW note
    XORLW d'59'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'14'
    MOVWF i
    MOVLW d'28'
    MOVWF j
    MOVLW d'11'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'60'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN     
    ;esp
    MOVFW note
    XORLW d'61'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN 
    ;-------------------------3era parte----------
;la3
    MOVFW note
    XORLW d'62'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'13'
    MOVWF i
    MOVLW d'27'
    MOVWF j
    MOVLW d'7'
    MOVWF k
    RETURN
    ;la3
    MOVFW note
    XORLW d'63'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'13'
    MOVWF i
    MOVLW d'27'
    MOVWF j
    MOVLW d'7'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'64'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
    ;esp
    MOVFW note
    XORLW d'65'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;si3
    MOVFW note
    XORLW d'66'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'12'
    MOVWF i
    MOVLW d'25'
    MOVWF j
    MOVLW d'18'
    MOVWF k
    RETURN
    ;si3
    MOVFW note
    XORLW d'67'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'12'
    MOVWF i
    MOVLW d'25'
    MOVWF j
    MOVLW d'18'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'68'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;sol3
    MOVFW note
    XORLW d'69'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'14'
    MOVWF i
    MOVLW d'28'
    MOVWF j
    MOVLW d'11'
    MOVWF k
    RETURN 
;esp
    MOVFW note
    XORLW d'70'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;la3
    MOVFW note
    XORLW d'71'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'13'
    MOVWF i
    MOVLW d'27'
    MOVWF j
    MOVLW d'7'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'72'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;si3
    MOVFW note
    XORLW d'73'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'12'
    MOVWF i
    MOVLW d'25'
    MOVWF j
    MOVLW d'18'
    MOVWF k
    RETURN
;---------------------si dooo
;do4
    MOVFW note
    XORLW d'74'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'11'
    MOVWF i
    MOVLW d'26'
    MOVWF j
    MOVLW d'14'
    MOVWF k
    RETURN
;si3
    MOVFW note
    XORLW d'75'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'12'
    MOVWF i
    MOVLW d'25'
    MOVWF j
    MOVLW d'18'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'76'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;sol3
    MOVFW note
    XORLW d'77'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'14'
    MOVWF i
    MOVLW d'28'
    MOVWF j
    MOVLW d'11'
    MOVWF k
    RETURN
    ;esp
    MOVFW note
    XORLW d'78'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;la3
    MOVFW note
    XORLW d'79'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'13'
    MOVWF i
    MOVLW d'27'
    MOVWF j
    MOVLW d'7'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'80'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;si3
    MOVFW note
    XORLW d'81'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'12'
    MOVWF i
    MOVLW d'25'
    MOVWF j
    MOVLW d'18'
    MOVWF k
    RETURN

;do4
    MOVFW note
    XORLW d'82'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'11'
    MOVWF i
    MOVLW d'26'
    MOVWF j
    MOVLW d'14'
    MOVWF k
    RETURN
;si3
    MOVFW note
    XORLW d'83'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'12'
    MOVWF i
    MOVLW d'25'
    MOVWF j
    MOVLW d'18'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'84'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;la3
    MOVFW note
    XORLW d'85'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'13'
    MOVWF i
    MOVLW d'27'
    MOVWF j
    MOVLW d'7'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'86'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;sol3
    MOVFW note
    XORLW d'87'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'14'
    MOVWF i
    MOVLW d'28'
    MOVWF j
    MOVLW d'11'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'88'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN
;la3
    MOVFW note
    XORLW d'89'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'13'
    MOVWF i
    MOVLW d'27'
    MOVWF j
    MOVLW d'7'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'90'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN    
;sol3
    MOVFW note
    XORLW d'91'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'1'
    MOVWF i
    MOVLW d'5'
    MOVWF j
    MOVLW d'10'
    MOVWF k
    RETURN
;esp
    MOVFW note
    XORLW d'92'
    BTFSS STATUS,Z 
    GOTO $+8
    MOVLW d'0'
    MOVWF i
    MOVLW d'0'
    MOVWF j
    MOVLW d'0'
    MOVWF k
    RETURN   
 
TITULO 
    BCF PORTA,0
    CALL time
    
    MOVLW 0x82		;LCD position
    MOVWF PORTB
    CALL exec
    BSF PORTA, 0
    CALL time
    
    
    MOVLW 'H'
    MOVWF PORTB
    CALL exec
    
    MOVLW 'I'
    MOVWF PORTB
    CALL exec
    
    MOVLW 'M'
    MOVWF PORTB
    CALL exec
    
    MOVLW 'N'
    MOVWF PORTB
    CALL exec
    
    MOVLW 'O'
    MOVWF PORTB
    CALL exec
    
    MOVLW ' '
    MOVWF PORTB
    CALL exec
    
    MOVLW 'A'
    MOVWF PORTB
    CALL exec
    
    MOVLW ' '
    MOVWF PORTB
    CALL exec
    
    MOVLW 'L'
    MOVWF PORTB
    CALL exec
    
    MOVLW 'A'
    MOVWF PORTB
    CALL exec
    
     BCF PORTA,0
    CALL time
    
    MOVLW 0xC3		;LCD position
    MOVWF PORTB
    CALL exec
    BSF PORTA, 0
    CALL time
    
    MOVLW 'A'
    MOVWF PORTB
    CALL exec
    
    MOVLW 'L'
    MOVWF PORTB
    CALL exec
    
    MOVLW 'E'
    MOVWF PORTB
    CALL exec
    
    MOVLW 'G'
    MOVWF PORTB
    CALL exec
    
    MOVLW 'R'
    MOVWF PORTB
    CALL exec
    
    MOVLW 'I'
    MOVWF PORTB
    CALL exec
    
    MOVLW 'A'
    MOVWF PORTB
    CALL exec
    
 
    RETURN
    
exec

    BSF PORTA,1		;exec
    CALL time
    BCF PORTA,1
    CALL time
    RETURN
    
time
    CLRF ij
    MOVLW d'10'
    MOVWF ji
ciclo    
    MOVLW d'80'
    MOVWF ij
    DECFSZ ij
    GOTO $-1
    DECFSZ ji
    GOTO ciclo
    RETURN
    


    tiempo:
    movlw d'201' ;establecer valor de la variable k
    movwf x
    xloop:
    nop
    decfsz x,f
    goto xloop
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    movlw d'252' ;establecer valor de la variable i
    movwf y
    yloop:
    nop ;NOPs de relleno (ajuste de tiempo)
    nop
    nop
    nop
    nop
    nop 
    movlw d'113' ;establecer valor de la variable j
    movwf p
    ploop:
    nop ;NOPs de relleno (ajuste de tiempo)
    movlw d'10' ;establecer valor de la variable k
    movwf a
    aloop:
    decfsz a,f
    goto aloop
    decfsz p,f
    goto ploop
    decfsz y,f
    goto yloop
    RETURN ;salir de la rutina de tiempo y regresar al
    ;valor de contador de programa
    END
