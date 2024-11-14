include irvine32.inc
.data

;We store the initial position

POS1 DWORD 10
POS2 DWORD 20
POS3 DWORD 30

POS4 DWORD 40
POS5 DWORD 50
POS6 DWORD 60

POS7 DWORD 70
POS8 DWORD 80
POS9 DWORD 90

UserPos DWORD ?
Buffer  BYTE 1 dup(?)
Winner DWORD ?

;Statments needed for the Output

line  BYTE "----------------------", 0
space BYTE "  ", 0
sep   BYTE "|", 0

Input  BYTE "Enter the Position: ", 0
head   BYTE "=============Welcome to Tic Tac Toe=============", 0
begin  BYTE "|       First Player: 1 && Second Player: 0    |", 0
bottom BYTE "================================================", 0

P1Turn  BYTE "Player 1 Turn!", 0
P0Turn  BYTE "Player 0 Turn!", 0
player1 BYTE "Congratulations! First Player (1) Wins...", 0
player2 BYTE "Congratulations! Second Player (0) Wins...", 0
drawMsg BYTE "Match Draw!", 0

.code 
main PROC
        ;Calling the Display Function
        call Menu
        call Display
        
        call GameStart

        
exit
main ENDP

;Function for the Menu
Menu PROC
         call crlf
         mov edx, OFFSET head
         call writeString
         call crlf

         mov edx, OFFSET begin
         call writeString
         call crlf

         mov edx, OFFSET bottom
         call writeString
         call crlf
         call crlf
ret
Menu ENDP

;Function To Start Game into Loop Until any of Player Win or Match is Draw
GameStart PROC USES eax ebx edx
        
              mov ecx, 9
              
              gameloop:
                      ; First player's turn

                      mov edx, OFFSET P1Turn
                      call writeString
                      call crlf

                      call TakeInputP1
                      sub ecx, 1        ; Decrement ecx after first player's turn

                      ;Clear Screen After every Input
                      call ClrScr

                      call Display      ; Display after player 1's turn

                      ;Check if Any Players Win or Not
                      
                      call DecideWinner

                     ; Check if it's time to end the game after player 1's turn
                     cmp ecx, 0        ; Compare ecx with 0
                     je endgame        ; If ecx == 0, end the game

                      mov edx, OFFSET P0Turn
                      call writeString
                      call crlf

                    ; Second player's turn
                    call TakeInputP2
                    sub ecx, 1        ; Decrement ecx after second player's turn

                    ;Clear Screen After every Input
                     call ClrScr

                    call Display      ; Display after player 2's turn
                    
                    ;Check if Any Players Win or Not
                      
                    call DecideWinner

                   ; Check if it's time to end the game after player 2's turn
                   cmp ecx, 0        ; Compare ecx with 0
                   je endgame        ; If ecx == 0, end the game

                   cmp ecx, 1        ;If ecx == 1 means only 1 turn left means
                   je drawMatch

                   jmp gameloop      ; Otherwise, continue the loop

       drawMatch:
                call crlf
                mov edx, OFFSET drawMsg
                call writeString

        endgame:
               ;Games end             
ret
GameStart ENDP

;Function to check all conditions and decide the player if any one win or the Match Draws
DecideWinner PROC USES eax
                 
                 ;Check upper 1st row
                 mov eax, POS1
                 cmp eax, POS2
                 jne nextCond2

                 cmp eax, POS3
                 je _final

                 ;Check 2nd row
                 nextCond2:
                          mov eax, POS4
                          cmp eax, POS5
                          jne nextCond3

                          cmp eax, POS6
                          je _final

                 ;Check 3rd row
                 nextCond3:
                          mov eax, POS7
                          cmp eax, POS8
                          jne nextCond4

                          cmp eax, POS9
                          je _final

                 ;Check 1st Col
                 nextCond4:
                          mov eax, POS1
                          cmp eax, POS4
                          jne nextCond5

                          cmp eax, POS7
                          je _final

                 
                 ;Check 2nd Col
                 nextCond5:
                          mov eax, POS2
                          cmp eax, POS5
                          jne nextCond6

                          cmp eax, POS8
                          je _final

                 ;Check 3rd row
                 nextCond6:
                          mov eax, POS3
                          cmp eax, POS6
                          jne nextCond7

                          cmp eax, POS9
                          je _final

                 
                 ;Check Left-Right Diagonal
                 nextCond7:
                          mov eax, POS1
                          cmp eax, POS5
                          jne nextCond8

                          cmp eax, POS9
                          je _final

                 
                 ;Check Right-Left Diagonal
                 nextCond8:
                          mov eax, POS7
                          cmp eax, POS5
                          jne _noResult

                          cmp eax, POS3
                          je _final
                          

                 _noResult:
                           jmp _functEnd
                      
                 _final:
                       mov Winner, eax
                       cmp eax, 1
                       jne _secondWin

                       mov edx, OFFSET player1
                       call writeString
                       mov ecx, 0                       ;To end the match loop so Games End
                       jmp _functEnd

                       _secondWin:
                                 mov edx, OFFSET player2
                                 call writeString 
                                 mov ecx, 0            ;To end the match loop so Games End

                 _functEnd:

ret
DecideWinner ENDP
;----------------------------------------------------------------------------------------

;Here we have a Display Function
;     1  |  2  |  3
;     -------------
;     4  |  5  |  6
;     -------------
;     7  |  8  |  9

Display PROC
            ;For POS1
            mov edx, OFFSET space
            call writeString

            mov eax, POS1
            call writeDec
            
            mov edx, OFFSET space
            call writeString

            mov edx, OFFSET sep
            call writeString

            mov edx, OFFSET space
            call writeString

            ;For POS2
            mov eax, POS2
            call writeDec
            
            mov edx, OFFSET space
            call writeString

            mov edx, OFFSET sep
            call writeString

            mov edx, OFFSET space
            call writeString

            ;For POS3
            mov eax, POS3
            call writeDec
            
            mov edx, OFFSET space
            call writeString

            ;Change the Line
            call crlf

            ;Print the sepration line
            mov edx, OFFSET line
            call writeString
            call crlf

            ;For POS4
            mov edx, OFFSET space
            call writeString

            mov eax, POS4
            call writeDec
            
            mov edx, OFFSET space
            call writeString

            mov edx, OFFSET sep
            call writeString

            mov edx, OFFSET space
            call writeString

            ;For POS5
            mov eax, POS5
            call writeDec
            
            mov edx, OFFSET space
            call writeString

            mov edx, OFFSET sep
            call writeString

            mov edx, OFFSET space
            call writeString

            ;For POS6
            mov eax, POS6
            call writeDec
            
            mov edx, OFFSET space
            call writeString

            ;Change the Line
            call crlf

            ;Print the sepration line
            mov edx, OFFSET line
            call writeString
            call crlf

            ;For POS7
            mov edx, OFFSET space
            call writeString

            mov eax, POS7
            call writeDec
            
            mov edx, OFFSET space
            call writeString

            mov edx, OFFSET sep
            call writeString

            mov edx, OFFSET space
            call writeString

            ;For POS8
            mov eax, POS8
            call writeDec
            
            mov edx, OFFSET space
            call writeString

            mov edx, OFFSET sep
            call writeString

            mov edx, OFFSET space
            call writeString

            ;For POS9
            mov eax, POS9
            call writeDec
            
            mov edx, OFFSET space
            call writeString

            ;Change the Line
            call crlf

ret
Display ENDP

;Function which input from the User and Store the New Values into GameBoard

TakeInputP1 PROC USES eax ebx ecx 
              mov edx, OFFSET Input
              call writeString

              call readDec
              mov UserPos, eax

              ;Here we write the conditions to place the user Input at their Desired position

              ;1st Compare with Position 10

              ;Ab woh check karega Postion 1 he ke ni
              cmp eax, POS1
              jne _next2

              ;Else we place the 1
              mov POS1, 1
              jmp _end

;-----------------------------------------------------------------------------------------

              ;2nd Compare with Position 20

              ;Ab woh check karega Postion 2 he ke ni
              _next2:
              cmp eax, POS2
              jne _next3

              ;Else we place the 1
              mov POS2, 1
              jmp _end

;-----------------------------------------------------------------------------------------

              ;3rd Compare with Position 30

              ;Ab woh check karega Postion 3 he ke ni
              _next3:
              cmp eax, POS3
              jne _next4

              ;Else we place the 1
              mov POS3, 1
              jmp _end

;-----------------------------------------------------------------------------------------

              ;4th Compare with Position 40

              ;Ab woh check karega Postion 4 he ke ni
              _next4:
              cmp eax, POS4
              jne _next5

              ;Else we place the 1
              mov POS4, 1
              jmp _end

;-----------------------------------------------------------------------------------------

              ;5th Compare with Position 50

              ;Ab woh check karega Postion 5 he ke ni
              _next5:
              cmp eax, POS5
              jne _next6

              ;Else we place the 1
              mov POS5, 1
              jmp _end

;---------------------------------------------------------------------------------------------

              ;6th Compare with Position 60

              ;Ab woh check karega Postion 6 he ke ni
              _next6:
              cmp eax, POS6
              jne _next7

              ;Else we place the 1
              mov POS6, 1
              jmp _end

;---------------------------------------------------------------------------------------------

              ;7th Compare with Position 70

              ;Ab woh check karega Postion 7 he ke ni
              _next7:
              cmp eax, POS7
              jne _next8

              ;Else we place the 1
              mov POS7, 1
              jmp _end

;---------------------------------------------------------------------------------------------

              ;8th Compare with Position 80

              ;Ab woh check karega Postion 8 he ke ni
              _next8:
              cmp eax, POS8
              jne _next9

              ;Else we place the 1
              mov POS8, 1
              jmp _end

;---------------------------------------------------------------------------------------------

              ;9th Compare with Position 90

              ;Ab woh check karega Postion 9 he ke ni
              _next9:
              cmp eax, POS9
              jne _end

              ;Else we place the 1
              mov POS9, 1
              jmp _end

;---------------------------------------------------------------------------------------------

             _end:
                 ; End of procedure                  

ret
TakeInputP1 ENDP

;Player 2 Input 

TakeInputP2 PROC USES eax ebx ecx
              mov edx, OFFSET Input
              call writeString

              call readDec
              mov UserPos, eax

              ;Here we write the conditions to place the user Input at their Desired position

              ;1st Compare with Position 10

              ;Ab woh check karega Postion 1 he ke ni
              cmp eax, POS1
              jne next2

              ;Else we place the 1
              mov POS1, 0
              jmp _end1

;-----------------------------------------------------------------------------------------

              ;2nd Compare with Position 20

              ;Ab woh check karega Postion 2 he ke ni
              next2:
              cmp eax, POS2
              jne next3

              ;Else we place the 1
              mov POS2, 0
              jmp _end1

;-----------------------------------------------------------------------------------------

              ;3rd Compare with Position 30

              ;Ab woh check karega Postion 3 he ke ni
              next3:
              cmp eax, POS3
              jne next4

              ;Else we place the 1
              mov POS3, 0
              jmp _end1

;-----------------------------------------------------------------------------------------

              ;4th Compare with Position 40

              ;Ab woh check karega Postion 4 he ke ni
              next4:
              cmp eax, POS4
              jne next5

              ;Else we place the 1
              mov POS4, 0
              jmp _end1

;-----------------------------------------------------------------------------------------

              ;5th Compare with Position 50

              ;Ab woh check karega Postion 5 he ke ni
              next5:
              cmp eax, POS5
              jne next6

              ;Else we place the 1
              mov POS5, 0
              jmp _end1

;---------------------------------------------------------------------------------------------

              ;6th Compare with Position 60

              ;Ab woh check karega Postion 6 he ke ni
              next6:
              cmp eax, POS6
              jne next7

              ;Else we place the 1
              mov POS6, 0
              jmp _end1

;---------------------------------------------------------------------------------------------

              ;7th Compare with Position 70

              ;Ab woh check karega Postion 7 he ke ni
              next7:
              cmp eax, POS7
              jne next8

              ;Else we place the 1
              mov POS7, 0
              jmp _end1

;---------------------------------------------------------------------------------------------

              ;8th Compare with Position 80

              ;Ab woh check karega Postion 8 he ke ni
              next8:
              cmp eax, POS8
              jne next9

              ;Else we place the 1
              mov POS8, 0
              jmp _end1

;---------------------------------------------------------------------------------------------

              ;9th Compare with Position 90

              ;Ab woh check karega Postion 9 he ke ni
              next9:
              cmp eax, POS9
              jne _end1

              ;Else we place the 1
              mov POS9, 0
              jmp _end1

;---------------------------------------------------------------------------------------------

             _end1:
                 ; End of procedure 
                                 

ret
TakeInputP2 ENDP

END main