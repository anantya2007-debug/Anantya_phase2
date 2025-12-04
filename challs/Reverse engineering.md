# worthy.knight

## Solution:
I was given a file `worthy.knight` so I first checked the file type. 
<img width="1211" height="73" alt="Screenshot 2025-12-04 at 10 36 47 AM" src="https://github.com/user-attachments/assets/55d67b09-9aa6-4c42-a54d-af18cf0146be" />
I then ran the file to see what I had to do and found that I needed a 10-character password to get the flag.  
<img width="1097" height="556" alt="Screenshot 2025-12-04 at 10 38 10 AM" src="https://github.com/user-attachments/assets/e6d37798-83cd-4315-9d55-c80cd879f64f" />

I then used `gdb` to go further and saw the following functions being used. 
<img width="412" height="340" alt="Screenshot 2025-12-04 at 10 44 23 AM" src="https://github.com/user-attachments/assets/fe1c16c1-4a06-4005-9e29-34b37b330dd2" />
I set a breakpoint at `fgets` so I could see when the input entered the program. This allowed me to inspect the memory right after the input was read. 
<img width="1271" height="812" alt="Screenshot 2025-12-04 at 10 46 15 AM" src="https://github.com/user-attachments/assets/3192eb6a-7f26-4e33-8050-65b78fc98dda" />

I then backtraced and got the frame inside the binary, which I could then disassemble around. 
<img width="1109" height="207" alt="Screenshot 2025-12-04 at 10 53 51 AM" src="https://github.com/user-attachments/assets/c530e3cf-bcff-4269-9e08-fa9dcdff6c85" />

From this segment, I got that the first and second byte XOR to be `0x24` and the second byte had to be `0x6a`, which is `j` in ASCII. This meant that the first byte would have to be `N`
<img width="518" height="180" alt="Screenshot 2025-12-04 at 10 58 41 AM" src="https://github.com/user-attachments/assets/ea85e885-2d4f-4104-b1b1-5ce109bc83bc" />

Similarly, the next two bytes would be `k` and `S` respectively from the screenshot below. 
<img width="621" height="170" alt="Screenshot 2025-12-04 at 11 01 48 AM" src="https://github.com/user-attachments/assets/163d862c-adca-4e12-b0c9-0ec431d979d5" />

For the next few bytes, they were swapped and sent to MD5. 

<img width="761" height="366" alt="Screenshot 2025-12-04 at 11 08 06 AM" src="https://github.com/user-attachments/assets/4f500167-4d33-4e52-8ec7-e4d6b74f3848" />

I inspected the meroy address given and got the MD5 hash. 
<img width="843" height="113" alt="Screenshot 2025-12-04 at 11 10 09 AM" src="https://github.com/user-attachments/assets/93f2bf23-c32e-4fe9-8a1e-7ff5e3b67750" />

I used the folowing python code to get the bytes from the hash 
```bash
import hashlib
import string

target = "33a3192ba92b5a4803c9a9ed70ea5a9c"

# we know MD5 input is [input[5], input[4]]
# so let's brute force (c4, c5) but hash (c5 + c4)

for a in string.printable:      
    for b in string.printable: 
        s = (b + a).encode()    
        if hashlib.md5(s).hexdigest() == target:
            print("FOUND pair:", a, b)
            print("MD5 input:", s.decode(errors="ignore"))
            exit()

```
From which i got the bytes `f` and `T`.

I finally got the last four characters the same way i got the first four. 
<img width="634" height="346" alt="Screenshot 2025-12-04 at 11 23 09 AM" src="https://github.com/user-attachments/assets/6d1b5805-0b5e-4657-8c01-aa2d5927ad7b" />

After putting it all together, i got the password as `NjkSfTYaIi`

I ran the file and input the password to finally get the flag. 
<img width="726" height="561" alt="Screenshot 2025-12-04 at 11 25 03 AM" src="https://github.com/user-attachments/assets/dd358e14-a331-4499-94cb-f7ae74f37924" />


## Flag:
```
KCTF{NjkSfTYaIi}
```
