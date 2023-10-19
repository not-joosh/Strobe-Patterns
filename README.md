# Strobe-Patterns
A project interfaced with an 8086 microprocessor, implemented in 8086 Assembly, and simulated in Proteus.

## Description
This assembly code project simulates three strobe patterns using the 8086 microprocessor. These patterns include a "WORM" strobe, "SNAKE" strobe, and a "ZIG-ZAG" strobe.

### Proteus Simulation (Hardware with Software)
![Traffic Lights Simulation](https://github.com/not-joosh/Traffic-Lights/assets/105687297/9f439f20-65cd-4847-b664-ddec8a5ad369)

### 8086 Assembly Software Script
```assembly
; TRAFFIC LIGHT EMBEDDED SYSTEM | INTERFACED WITH 8086 MICROPROCESSOR
DATA SEGMENT
   PORTA    EQU 0F0H ; 0C6H
   PORTB    EQU 0F2H ; 0CEH   
DATA ENDS

CODE SEGMENT PUBLIC 'CODE'
   ASSUME CS:CODE
START:
   TESTLOOP:   
      ; BUTTON INPUTS
      MOV DX, PORTB
      IN AL, DX  
      TEST AL, 01H
      JE DO_TRAFFIC
      TEST AL, 02H
      JE DO_HAZARD
      TEST AL, 04H
      JE DO_INVERTED
      TEST AL, 08H
      JE DO_TURNOFF
      ; NO CONDITIONS SATISFIED
      MOV DX, PORTA
      MOV AL, 00000111B
      OUT PORTA, AL 
      JMP NEXTPHASE

   DO_TRAFFIC:
      CALL TRAFFIC
      JMP NEXTPHASE  
   DO_HAZARD:
      CALL HAZARD
      JMP NEXTPHASE  
   DO_INVERTED:
      CALL INVERTED
      JMP NEXTPHASE  
   DO_TURNOFF:
      CALL TURNOFF
      JMP NEXTPHASE

   NEXTPHASE:
      JMP TESTLOOP

; PROCEDURES
TRAFFIC:
   MOV DX, PORTA
   MOV AL, 00000001B
   OUT PORTA, AL           
   CALL DELAY
   MOV DX, PORTA
   MOV AL, 00000010B
   OUT PORTA, AL  
   CALL DELAY
   MOV DX, PORTA
   MOV AL, 00000100B
   OUT PORTA, AL
   CALL DELAY
   RET

HAZARD:
   MOV DX, PORTA
   MOV AL, 00000111B
   OUT PORTA, AL           
   CALL DELAY
   MOV DX, PORTA
   MOV AL, 00000000B
   OUT PORTA, AL           
   CALL DELAY
   RET

INVERTED:
   MOV DX, PORTA
   MOV AL, 00000100B
   OUT PORTA, AL           
   CALL DELAY
   MOV DX, PORTA
   MOV AL, 00000010B
   OUT PORTA, AL  
   CALL DELAY
   MOV DX, PORTA
   MOV AL, 00000001B
   OUT PORTA, AL
   CALL DELAY
   RET

TURNOFF:
   MOV DX, PORTA
   MOV AL, 00000000B
   OUT PORTA, AL           
   CALL DELAY
   RET

SKIPDELAY: 
   ; Delay function
   MOV CX, 65000d
   DELAY_LOOP:    
      LOOP DELAY_LOOP
   RET

ENDLESS:
   JMP ENDLESS

CODE ENDS
END START
```
