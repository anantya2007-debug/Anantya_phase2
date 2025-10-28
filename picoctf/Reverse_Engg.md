# 2. ARMssembly 1

For what argument does this program print `win` with variables 58, 2 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution:
I opened the given file and saw that the code was written in assembly language. I searched up the syntax of the keywords used and analyzed the code to see what would trigger the ```You win``` function. 

```bash
	.arch armv8-a
	.file	"chall_1.c"
	.text
	.align	2
	.global	func
	.type	func, %function
func:
	sub	sp, sp, #32 //Make space on the stack
	str	w0, [sp, 12] //Stores input at memory location [sp+12]
	mov	w0, 58   //transfer the value 58 into w0
	str	w0, [sp, 16] //Store 58 at memory location [sp+16]
	mov	w0, 2 //Moves 2 into w0
	str	w0, [sp, 20] //Store 2 at [sp+20]
	mov	w0, 3 //Move 3 into w0
	str	w0, [sp, 24] // Store 3 at [sp+24]
	ldr	w0, [sp, 20] //gets w0 = 2 from memory
	ldr	w1, [sp, 16] //gets w1 = 58 from memory
	lsl	w0, w1, w0 //w0 = w1 << w0  (58 << 2)
	str	w0, [sp, 28] //Stores result (232) at [sp+28]
	ldr	w1, [sp, 28] //w1 = 232
	ldr	w0, [sp, 24] //w0 = 3
	sdiv	w0, w1, w0 //w0 = w1 / w0  (232 ÷ 3)
	str	w0, [sp, 28] //Store result (77) at [sp+28]
	ldr	w1, [sp, 28] //w1 = 77
	ldr	w0, [sp, 12] //w0 = number input number from user
	sub	w0, w1, w0 //w0 = w1 - w0
	str	w0, [sp, 28] //Store final result at [sp+28]
	ldr	w0, [sp, 28] //w0 = the final result
	add	sp, sp, 32 //basically returns the space made at the beginning
	ret
	.size	func, .-func
	.section	.rodata
	.align	3
.LC0:
	.string	"You win!"
	.align	3
.LC1:
	.string	"You Lose :("
	.text
	.align	2
	.global	main
	.type	main, %function
main:
	stp	x29, x30, [sp, -48]!
	add	x29, sp, 0
	str	w0, [x29, 28]
	str	x1, [x29, 16]
	ldr	x0, [x29, 16]
	add	x0, x0, 8
	ldr	x0, [x0]
	bl	atoi
	str	w0, [x29, 44]
	ldr	w0, [x29, 44]
	bl	func
	cmp	w0, 0 //compares result with 0
	bne	.L4
	adrp	x0, .LC0 //if not equal to zero then it calls LC1
	add	x0, x0, :lo12:.LC0
	bl	puts
	b	.L6
.L4:
	adrp	x0, .LC1
	add	x0, x0, :lo12:.LC1
	bl	puts
.L6:
	nop
	ldp	x29, x30, [sp], 48
	ret
	.size	main, .-main
	.ident	"GCC: (Ubuntu/Linaro 7.5.0-3ubuntu1~18.04) 7.5.0"
	.section	.note.GNU-stack,"",@progbits
```

From this, I got that 77-user_input had to be equal to 0, which meant the answer was 77.
I then converted 77 from decimal to hexadecimal and put it into picoCTF{} to get the flag. 

## Flag:
```
picoCTF{0000004d}
```

## Concepts learnt:
- ```mov``` moves the specified value into the memory (Example: ```mov	w0, 58   //transfer the value 58 into w0```
- ```str``` stores the specified value into a specified memory location (Example: ```str	w0, [sp, 16] //Store 58 at memory location [sp+16]```
- ```ldr``` transfers data from memory to register (Example: ```ldr	w0, [sp, 20] //gets w0 = 2 from memory```
- ```LSL``` instruction performs a bitwise operation that shifts the bits of a register to the left by a specified number of positions (Example: ```lsl	w0, w1, w0  // w0 = w1 << w0  (58 << 2)```
- ```sdiv``` is used to division (Example: ```sdiv	w0, w1, w0 //w0 = w1 / w0  (232 ÷ 3)```)
- ```sub``` is used for subtraction (Example: ```sub	w0, w1, w0 //w0 = w1 - w0```


## Resources:
- [https://how.dev/answers/what-is-basic-syntax-in-assembly](url)

  

# 3. Vault door 3 

This vault uses for-loops and byte arrays. The source code for this vault is here: VaultDoor3.java
Make a table that contains each value of the loop variables and the corresponding buffer index that it writes to.
<img width="1512" height="982" alt="Screenshot 2025-10-25 at 6 12 44 PM" src="https://github.com/user-attachments/assets/b3b3c811-70e3-413d-88c4-4bacf030ed7d" />


## Solution 
I made the table mentioned in the hints and then put all the characters together to get the flag.

<img width="409" height="638" alt="Screenshot 2025-10-25 at 7 03 43 PM" src="https://github.com/user-attachments/assets/da23d663-2948-44d9-aed2-6f2831a0fd61" />


## Flag:
```
picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_79958f}
```
