# MASM-GRADES-TO-STARS
This program takes in grades and puts out a certain number of stars, every 10 points, a star is displayed.


TITLE MASM Template						(main.asm)

INCLUDE Irvine32.inc
.data

askgrades byte "Please enter 5 grades!",0ah,0dh,0
printStars byte "*",0

;ARRAY USED TO STORE 5 GRADES
gradesArr dword 5 dup(0)

;VARIABLE THAT HOLDS INPUT GRADES
grade dword 0

.code

getData proc
	;PREPAIRING ARRAY
	mov esi, offset gradesArr
	;SETTING ECX TO 5 CONTROLS L1 LOOP
	mov ecx, 5
	;ASKING FOR 5 GRADES FROM THE USER
	mov edx, offset askgrades	
	call writestring

	;TAKING IN 5 VARIABLES FROM THE USER
	L1:
		call readint
		;[ESI] IS A REFERENCE TO THIS LOCATION IN MEMORY
		;MOVING USER INPUT VALUE TO ESI LOCATION
		mov [esi], eax

		;ADDING 4 TO ESI ALLOWS US TO MOVE TO THE
		;NEXT LOCATION IN THE ARRAY
		add esi, 4

		;TAKES US UP TO L1 LOOP AGAIN UNTIL ECX = 0
		loop L1
ret
getData endp

displayData proc
	
	;INITIALIZING ARRAY AND SETTING LOOP ECX
	;INITIALIZING ECX TO 0 AS LOOP WILL COUNT UP TO 5
	mov ecx, 0
	mov esi, offset gradesArr

	;EBX WILL BE FOUND IN L1, COMPARING grade
	;IF grade IS LESS THAN 10, MOVE UP TO L2
    mov ebx, 10

	;L2 IS MY OUTER LOOP
	L2:
		;COMPARING VALUE STORED IN ECX TO 5
		cmp ecx, 5	
		;IF ECX EQUALS 5 JUMP TO DONE
        je done
		;MOVE WHATEVER IS IN ESI MEM LOC TO EAX
		mov eax, [esi]
		;STORE EAX VALUE TO grade
		mov grade, eax
		;MOVE ON TO NEXT SPOT IN ARRAY
		add esi, 4

		;L1 IS MY INNER LOOP
		L1:
			;COMPARING grade VALUE TO EBX
			cmp grade, ebx
			;IF grade IS LESS THAN EBX (10)
			;JUMP TO L1Done
			jl L1Done
			;SUBTRACTING 10 FROM VALUE IN grade
			sub grade, 10
			mov edx, offset printStars
			call writeString
			;CALLING BACK TO BEGINNING OF L1
			jmp L1

			L1Done:
				call crlf
				;TRIED USING Loop L2 INSTRUCTION BUT
				;BUT WAS NEVER ABLE TO GET IT TO LOOP PROPERLY
				inc ecx
				jmp L2
	done:
ret
displayData endp

main PROC
	call getData
	call displayData
	call readInt
	exit
main ENDP
END main

