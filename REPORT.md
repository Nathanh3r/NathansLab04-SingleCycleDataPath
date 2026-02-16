#REPORT.md

name: Nathan Herrera 
email: nherr044@ucr.edu

##Issues and Bugs 
Some issues and bugs inside of the lab were found inside of digital. When trying to pass in wires by connecting them i kept getting an error that
I was putting 32 bits when the registers called for 8 bits. To solve this I was able to use splitters to cut down the number of bits going into the registers. 

Furthermore the shift left by 2 coming from the sign extended portion was also offering me some difficulty. This was due to not being aware how to accomplish this
using the tools we used in lab meeting. To solve this problem I was able to first split the original bits coming into 0-29 and 30-31. After doing this I used another splitter and took the bits from 
0-29 and made another splitter with input of 0-29. I then I also took 2 bits into the splitter and made them both zero.

Finally this was merged and then exported to the ALU to be able to finally do the shifting. Aside from this the lab manual was used to 
keep the wires from going where they shouldnt be. The outputs on the right side of the circuit was also very helpful to decode what wire was
supposed to be changed and fixed.


#Hand Assembly
1) and $t0, $t1, $t2
opcode=000000
rs=$t1 = 9 then we have 01001
rt = $t2 = 10 which gives 01010
shamt= then 00000
funct(AND) = 100100

which gives binary of: 000000 01001 01010 01000 00000 100100 and hex opf 0x012A4024

2) addi $t3, $t4, 123
this is a i type instruction so we need immediate
opcode =001000
rs = $t4 = 12 which gives 01100
rt = $t3 = 11 which is then 01011
imm= 123 which gives 0000000001111011

finally giving binary of 
001000 01100 01011 0000000001111011 and hex of 0x218B007B

3) lw $t5, 135($t1)
opcode (lw) = 100011
rs = $t1 = 9 → 01001
rt = $t5 = 13 → 01101
imm = 135 → 0000000010000111

giving us binary 
100011 01001 01101 0000000010000111 and hex 0x8D2D0087

4) sw $t3, 136($t1)
opcode (sw) = 101011
rs = $t1 = 9 → 01001
rt = $t3 = 11 → 01011
imm = 136 → 0000000010001000

giving binary of 
101011 01001 01011 0000000010001000 and hex of 0xAD2B0088

5) beq $t3, $zero, -10
for -10 we must have
that
10 = 0000 0000 0000 1010
take this then invert 1111 1111 1111 0101

add 1 
1111 1111 1111 0110

opcode (beq) = 000100
rs = $t3 = 11 → 01011
rt = $zero = 0 → 00000
imm = 1111111111110110

which gives binary of 
000100 01011 00000 1111111111110110 and hex of 0x1160FFF6

Trace of init.asm 


when starting we have all registers starting at 0. we know this through init.hex that 
Memory[31] = 0x56
and Memory[132]=0x00 when starting 

1) lw $v0, 31($zero)
the effective address is 31 due to $zero=0 and the value in there goes into $v0 so now it holds 0x56

2. add $v1, $v0, $v0
here we have the adding of 0x56 +0x56 into $v1 giving us $v1 = 0xAC

3. sw $v1, 132($zero)
we then store the val we just computed into the memeory address 132
so this now holds this value

4. sub $a0, $v1, $v0
then we now subtract the 0x56 from the sum of 0xAC found earlier into $a0

5. addi $a1, $v1, 12
here we have a addi so we must add immediate 12 to 0xAc. which is 0x0C in hexadecimal
so we have 0xB8 inside of $a1.


6. and $a2, $a1, $v1
Bitwise AND of 0xB8 and 0xAC gives 0xA8 and we put this into $a2.


7. or $a3, $a2, $v0
Bitwise OR of 0xA8 and 0x56 gives 0xFE which is then stored into $a3.


8. nor $t0, $a2, $v0
First we do or which gives us 0xFE after or'ing both $a2 and $v0, then we do the bitwise not of 0xFFFFFF01. Then it is stored into the $t0


9. slt $a2, $a1, $a0
Compare 0xB8 (184) and 0x56 (86).
Since 184 is not less than 86, result is 0 and is then stored into $a2=0.

10. beq $a2, $zero, -8

We know that $a2 equals 0, so this goes ahead and satisfies the branch.
The PC goes back by the offset, but registers do not change they stay the same through this command.

11.lw $t0, 132($zero)

Loads the value at memory address 132 which was stored as 0xAC earlier in the program into $t0.

So finally we have the final registers of 
$v0 = 0x56
$v1 = 0xAC
$a0 = 0x56
$a1 = 0xB8
$a2 = 0x00
$a3 = 0xFE
$t0 = 0xAC

Lastly memory at 132 holds 0xAC




