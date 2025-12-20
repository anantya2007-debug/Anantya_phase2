
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

- **mov** : to load data into registers (mov rax, 0x539);  0x539-immediate value; It copies the data but doesn't move it 
    1. From higher to lower: the extra just gets zeroed out
- **movsx** : sign-extended move; presevers two's compliment value
- **xchg** : swaps value of two register

- **rip** (special register): address of next instruction 

<img width="494" height="186" alt="Screenshot 2025-12-20 at 11 41 37 AM" src="https://github.com/user-attachments/assets/760382f2-a1e4-47b9-bfbc-74e48a934a3d" />

  
