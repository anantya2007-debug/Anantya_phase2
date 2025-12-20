
- CPU -> Logic gates 
<img width="570" height="268" alt="Screenshot 2025-12-19 at 3 25 25 PM" src="https://github.com/user-attachments/assets/75baebfd-faa1-4e29-a26a-c473daea3f55" />

- Assembly: text representation of binary (both equivalent); CPU architecture dependent

- Varients: x86, arm, ppc, mips, risc-v, pdp-11 (x86-Intel from now onwards)

### Data:
- binary digit -> bit
- ASCII -> UTF-8
- 8 bit byte 
<img width="228" height="79" alt="Screenshot 2025-12-19 at 4 26 50 PM" src="https://github.com/user-attachments/assets/33cadb78-9ddb-48e5-985b-774004c655ef" />

### Registers: 
- rapid access to data by CPU through the register files
- temporary storage
- stores the address of the next instruction
- amd64: rax, rcx, rdx, rbx, rsp, rbp, rsi, rdi, r8, r9, r10, r11 (general-purpose registers)
- 64-bit architecture -> each register will hold 64 bits
- partial register access: ah, al, ax, eax, rax
<img width="209" height="58" alt="Screenshot 2025-12-20 at 11 31 17 AM" src="https://github.com/user-attachments/assets/ce49c969-06c4-498f-83c2-ba0dcc580f85" />
<img width="520" height="278" alt="Screenshot 2025-12-20 at 5 57 25 PM" src="https://github.com/user-attachments/assets/1ad2cb1a-dd06-4fb4-84f0-0ede7458bd10" />
<img width="498" height="267" alt="Screenshot 2025-12-20 at 5 49 35 PM" src="https://github.com/user-attachments/assets/f4eb6963-3613-4f56-ac61-d01f12238280" />

- **mov** : to load data into registers (mov rax, 0x539);  0x539-immediate value; It copies the data but doesn't move it 
    1. From higher to lower: the extra just gets zeroed out
- **movsx** : sign-extended move; presevers two's compliment value
- **movzx** : move with zero extended; moves a smaller value and fills the rest of the destination register with zeros
- **xchg** : swaps value of two register
- **rip** (special register): address of next instruction; relative addressing  

<img width="494" height="186" alt="Screenshot 2025-12-20 at 11 41 37 AM" src="https://github.com/user-attachments/assets/760382f2-a1e4-47b9-bfbc-74e48a934a3d" />

- **add** reg1, reg2 -> reg1 += reg2
- **sub** reg1, reg2 -> reg1 -= reg2
- **imul** reg1, reg2 -> reg1 *= reg2 ; mul (unsigned multiply) and imul (signed multiply)
- **div** reg -> rax = rdx:rax / reg (where rax: quotient, rdx: remainder, reg: divisor)
- **shl** al, 1 -> shifts everything to the left by 1
- **shr** al, 1 -> shifts everything to the right by 1

### Memory:
- Stack
    1. **push**
    2. **pop**
- **rsp** : stores address of stack
- **[ ]** : memory address
- modern systems store data backwards, in **little endian**
- **lea** : load effective address; address specified by its first operand into the register specified by its second operand; can directly access the rip register 

### Control flow:
- **jmp** : jumps to the specified label; interrupts sequence 
<img width="189" height="203" alt="Screenshot 2025-12-20 at 2 45 01 PM" src="https://github.com/user-attachments/assets/e74309fa-0ccf-4d05-bc86-4cc5ec41b43c" />

- conditional jumps check conditions stored in the register **rflags**

- Main conditional flags:
     1. Carry Flag: was the 65th bit 1?
     2. Zero Flag: was the result 0?
     3. Overflow Flag: did the result "wrap" between positive to negative?
     4. Sign Flag: was the result's signed bit set (i.e., was it negative)?

- Types of jumps
     1. Relative jumps: jump + or - the next instruction.
     2. Absolute jumps: jump to a specific address.
     3. Indirect jumps: jump to the memory address specified in a register.

- Relative jumps
     1. Labels: Labels act as named placeholders for code locations, letting the assembler automatically calculate jump offsets for you.
     2. nop: nop is a one-byte instruction that does nothing and is commonly used as filler to control instruction spacing.
     3. .rept: .rept repeats an instruction a specified number of times, making it easy to generate large blocks of nops.

- **Loop**:
   ```bash
   mov rax, 0
   LOOP_HEADER:
   inc rax
   cmp rax, 10
   jb LOOP_HEADER
   ```
- **call** saves the place so **ret** can return to that place
     ```bash
     call FUNC_CHECK   #calls the function

     FUNC_CHECK:
     mov ax, 1337
     ret               #function edns and returns back to where it was called
     ```

### System calls: 
- read(): 0
- write(): 1
- **syscall**

### Disassembling a program 
``` objdump -M intel -d prog_name ```

### GDB
debugging 

### strace 
how program is interacting with the OS 

### Rappel 
explore the effects of instructions 

 ---

```bash
import pwn

pwn.context.update(arch="amd64")
output = pwn.process("/challenge/run")
output.write(pwn.asm("""

# Write your assembly code here

"""))
print(output.readallS())
```

 ---
```bash
if x is even then
  y = 1
else
  y = 0
```
```bash
and rax, rdi
and rax, 1
xor rax, 1
```
 ---
```bash 
movzx rax, byte ptr [0x404000]
movzx rbx, word ptr [0x404000]
mov ecx, dword ptr [0x404000]
mov rdx, qword ptr [0x404000]
```
