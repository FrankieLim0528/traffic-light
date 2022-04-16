;************************************************************************************
; A program to control a cross junction traffic
;
; BASIC Features:
; - Equal chances for users to exit and cross the junction without causing accident / havoc
; - Controlled by tri-colour traffic lights indicator
;   - RED     (STOP)
;   - YELLOW  (Prepare to STOP)
;   - GREEN   (Permission to GO)
;
; EXTRA Features SUGGESTIONS:
; - Emergency buttons for AMBULANCE and POLICE CARS (DONE)
; - Pedestrians press a button to cross the ROAD
; - Count Down Indicator (DONE)

		ORG	00h
		AJMP	MAIN

;************************************************************************************
; Configure all the ports before running the program
;
; PORT 0 (0-3) RED
; PORT 0 (4-7) YELLOW
;
; PORT 1 (0-3) GREEN
;
; PORT 2 (0-3) PEDESTRIAN CROSSING LIGHTS
;        0,1 - RED
;	 2,3 - GREEN
; PORT 2 (4-5) PEDESTRIAN CROSSING BUTTONS
; PORT 2 (6-7) EMERGENCY BUTTONS (TOP and BOTTOM ROAD)
;
; PORT 3 (0-7) COUNT DOWN INDICATORS

MAIN:		MOV	A,#0F0h		; 1111 0000
		MOV	P0,A		; Turn off all YELLOW lights, Turn on all RED lights
		MOV	A,#0FFh		; 1111 1111
		MOV	P1,A		; Turn off all GREEN lights, Set P1.4 to P1.7 as input ports for EMERGENCY BUTTONS
		MOV	A,#0FCh		; 1111 1100
		MOV	P2,A		; Set PEDESTRIAN CROSSING LIGHTS P2.0 to P2.3
					; Set P2.4 to P2.7 as input ports for PEDESTRIAN CROSSING BUTTONS and EMERGENCY BUTTONS
		MOV	A,#00h		; 0000 0000
		MOV	P3,A		; set PORT 3 as ouput port (7 segment display)
		MOV	DPTR,#SEG	; Move table address to Data Pointer
		MOV	B,#00h		; initialize DELAY counter
		AJMP	TOP


;************************************************************************************
; ROAD (TOP -> RIGHT -> BOTTOM -> LEFT -> TOP)
;
; 1. Turn off RED
; 2. Turn on GREEN
; 3. TIMER count down for 9 seconds
; 4. Turn YELLOW
; 5. TIMER count down for 3 seconds
; 6. Turn RED
; 7. Wait for 3 seconds
; 8. During DELAY
;     Check if EMERGENCY BUTTONS are pressed (TOP and BOTTOM)
;     Check if PEDESTRIAN CROSSING BUTTONS are pressed (LEFT and RIGHT)
; 9. Proceed to the next ROAD

TOP:		;MOV	A,#0F1h		; 1111 0001
		;MOV	P0,A		; turn off RED at TOP
		;MOV	A,#0FEh		; 1111 1110
		;MOV	P1,A		; Turn on GREEN at TOP

		SETB	P0.0			; Turn off RED
		CLR	P1.0			; Turn on GREEN
		MOV	B,#09h
		ACALL	DELAY			; DELAY 9 seconds
		SETB	P1.0			; Turn off GREEN
		CLR	P0.4			; Turn on YELLOW
		MOV	B,#03h
		ACALL	DELAY			; DELAY 3 seconds
		SETB	P0.4			; Turn off YELLOW
		CLR	P0.0			; Turn on RED
		MOV	B,#03h
		ACALL	DELAY			; DELAY 3 seconds
		AJMP	RIGHT			; Proceed to RIGHT ROAD

RIGHT:		SETB	P0.1			; Turn off RED
		CLR	P1.1			; Turn on GREEN
		MOV	B,#09h
		ACALL	DELAY			; DELAY 9 seconds
		SETB	P1.1			; Turn off GREEN
		CLR	P0.5			; Turn on YELLOW
		MOV	B,#03h
		ACALL	DELAY			; DELAY 3 seconds
		SETB	P0.5			; Turn off YELLOW
		CLR	P0.1			; Turn on RED
		MOV	B,#03h
		ACALL	DELAY			; DELAY 3 seconds
		JNB	P2.4,WALK_RIGHT		; Check if PEDESTRIANN CROSSING LIGHTS BUTTON is pressed
		AJMP	BOTTOM			; Proceed to BOTTOM ROAD

BOTTOM:		SETB	P0.2			; Turn off RED
		CLR	P1.2			; Turn on GREEN
		MOV	B,#09h
		ACALL	DELAY			; DELAY 9 seconds
		SETB	P1.2			; Turn off GREEN
		CLR	P0.6			; Turn on YELLOW
		MOV	B,#03h
		ACALL	DELAY			; DELAY 3 seconds
		SETB	P0.6			; Turn off YELLOW
		CLR	P0.2			; Turn on RED
		MOV	B,#03h
		ACALL	DELAY			; DELAY 3 seconds
		AJMP	LEFT			; Proceed to LEFT ROAD

LEFT:		SETB	P0.3			; Turn off RED
		CLR	P1.3			; Turn on GREEN
		MOV	B,#09h
		ACALL	DELAY			; DELAY 9 seconds
		SETB	P1.3			; Turn off GREEN
		CLR	P0.7			; Turn on YELLOW
		MOV	B,#03h
		ACALL	DELAY			; DELAY 3 seconds
		SETB	P0.7			; Turn off YELLOW
		CLR	P0.3			; Turn on RED
		MOV	B,#03h
		ACALL	DELAY			; DELAY 3 seconds
		JNB	P2.5,WALK_LEFT		; Check if PEDESTRIANN CROSSING LIGHTS BUTTON is pressed
		AJMP	TOP			; Back to TOP ROAD


;************************************************************************************
; PEDESTRIAN CROSSING LIGHTS SUBROUTINE
;
; PEDESTRIAN CROSSING LIGHTS are located at LEFT and RIGHT ROAD
;
; 1. After button is pressed, DELAY for 3 seconds
; 2. GREEN light turn ON for 8 seconds
; 3. Turn off GREEN light
; 	COUNT DOWN TIMER

WALK_RIGHT:	SETB	P2.0			; Turn off RED
		CLR	P2.2			; Turn on GREEN
		MOV	B,#05h
		ACALL	DELAY			; DELAY for 5 seconds
		SETB	P2.2			; Turn off GREEN
		CLR	P2.0			; Turn on RED
		MOV	B,#03h
		ACALL	DELAY			; DELAY for 3 seconds
		AJMP	BOTTOM			; Continue to BOTTOM ROAD

WALK_LEFT:	SETB	P2.1			; Turn off RED
		CLR	P2.3			; Turn on GREEN
		MOV	B,#05h
		ACALL	DELAY			; DELAY for 5 seconds
		SETB	P2.3			; Turn off GREEN
		CLR	P2.1			; Turn on RED
		MOV	B,#03h
		ACALL	DELAY			; DELAY for 3 seconds
		AJMP	TOP			; Continue to TOP ROAD

;************************************************************************************
; EMERGENCY SUBROUTINE
;
; EMERGENCY buttons are located at TOP and BOTTOM ROAD
; Check if EMERGENCY buttons are pressed during DELAY subroutines
;
; EMERGENCY STATE:
; 1. Turn off all GREEN and YELLOW lights
; 2. Turn on all RED lights
; 3. Wait for 3 seconds

; EMERGENCY STATE
EMERGENCY:	MOV	A,#0Fh			; 0000 1111
		MOV	P1,A			; Turn off all GREEN lights
		MOV	A,#0F0h			; 1111 0000
		MOV	P0,A			; Turn off all YELLOW lights, Turn on all RED lights
		MOV	B,#03h
		ACALL	DELAY			; DELAY 3 seconds
		RET

; EMERGENCY AT TOP ROAD
EMERG_TOP:	ACALL EMERGENCY
		SETB	P0.0			; Turn off RED at TOP ROAD
		CLR	P1.0			; Turn on GREEN at TOP ROAD
		MOV	B,#09h
		ACALL	DELAY			; DELAY 9 seconds
		SETB	P1.0			; Turn off GREEN at TOP ROAD
		CLR	P0.4			; Turn on YELLOW at TOP ROAD
		MOV	B,#03h
		ACALL	DELAY			; DELAY for 3 seconds
		SETB	P0.4			; Turn off YELLOW
		CLR	P0.0			; Turn on RED
		MOV	B,#03h
		ACALL	DELAY			; DELAY for 3 seconds
		AJMP	RIGHT			; Continue to RIGHT ROAD

; EMERGENCY AT BOTTOM ROAD
EMERG_BOTTOM:	ACALL	EMERGENCY
		SETB	P0.2			; Turn off RED at BOTTOM ROAD
		CLR	P1.2			; Turn on GREEN at BOTTOM ROAD
		MOV	B,#09h
		ACALL	DELAY			; DELAY 9 seconds
		SETB	P1.2			; Turn off GREEN at BOTTOM ROAD
		CLR	P0.6			; Turn on YELLOW at BOTTOM ROAD
		MOV	B,#03h
		ACALL	DELAY			; DELAY for 3 seconds
		SETB	P0.6			; Turn off YELLOW
		CLR	P0.2			; Turn on RED
		MOV	B,#03h
		ACALL	DELAY			; DELAY for 3 seconds
		AJMP	LEFT			; Continue to LEFT ROAD


;************************************************************************************
; DELAY SUBROUTINE
;
; Control DELAY time in seconds using value stored in B
; #02h == 2 seconds
;
; USAGE:
; To delay by 1 second,
;  MOV	  B,#01h
;  ACALL  DELAY
;
; YELLOW     	   	(3 seconds)
; GREEN      	   	(9 seconds)
; All RED    	   	(3 seconds)
; Count Down Timer 	(1 seconds)
; Pedestrian Crossing 	(5 seconds)

; DELAY 1 SECOND
DELAY_1S:	MOV	R1,#03h
LOOP1:		DJNZ	R1,LOOP1
		RET

; DELAY FOR TRAFFIC LIGHTS AND COUNT DOWN TIMER
DELAY:		MOV	R2,B
TIMER:		ACALL	COUNTER			; display current time remaining on TIMER
LOOP2:		ACALL	DELAY_1S		; DELAY 1 second
		DEC	R2			; Decrement R2 by 1
		JNB	P2.6,EMERG_TOP		; Check if EMERGENCY BUTTON is pressed
		JNB	P2.7,EMERG_BOTTOM
		CJNE	R2,#00h,TIMER		; Continue to loop if time remaining is not 0
		ACALL	COUNTER
		ACALL	DELAY_1S
		MOV	A,#00h
		MOV	P3,A			; Clear the 7 SEGMENT DISPLAY if time left is 0
		RET


;************************************************************************************
; COUNT DOWN TIMER SUBROUTINE
;
; To show number pattern on 7 SEGMENT LED DISPLAY for every second

COUNTER:	MOV	A,R2
		MOVC	A,@A+DPTR
		MOV	P3,A
		RET


;************************************************************************************
; lookup table for 7 segment display pattern (0-F)

SEG:	DB	3Fh,06h,5Bh,4Fh,66h,6Dh,7Dh,07h,7Fh,6Fh,77h,7Ch,39h,5Eh,79h,71h

	END