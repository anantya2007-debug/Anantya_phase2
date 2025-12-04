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

## Resources:
- [https://www.youtube.com/watch?v=1iptoUKXrcI](url)


# Time 

## Solution:

I first ran `file time` to see what the file type is.
<img width="724" height="87" alt="Screenshot 2025-12-04 at 12 51 32 PM" src="https://github.com/user-attachments/assets/5a3d3e46-4b13-4baa-8df0-973466bb5e99" />

I then ran the file, and it opened to a number-guessing game. 

<img width="701" height="228" alt="Screenshot 2025-12-04 at 12 52 32 PM" src="https://github.com/user-attachments/assets/ce1dfdd8-e51b-45be-b93a-12341ecb8e06" />

I used `disassemble main`.
<img width="719" height="147" alt="Screenshot 2025-12-04 at 1 32 01 PM" src="https://github.com/user-attachments/assets/2a772ed6-b083-49e8-b3d1-69045e5ec20a" />

From the screenshot, I saw that it took the input number and compared it to the secret number. If the two numbers were the same, then it would return the flag. Otherwise, it would jump (`jne`) it and say that the number given was the wrong answer. 

I used `break *0x4009fc` to set a breaking point so that it breaks right before the comparison executes. So at this point, `%eax` holds the input number and `-0xc(%rbp)` holds the secret random number. So I realised I could just print out this value to see the secret number. 
<img width="783" height="334" alt="Screenshot 2025-12-04 at 1 40 28 PM" src="https://github.com/user-attachments/assets/a40b7c3d-188b-4cbd-851a-b7246f16edc9" />

This worked in the scenario of me wanting to find the secret number, but I also wanted to get the flag message printed, so I needed to set the break earlier on. 

<img width="672" height="114" alt="Screenshot 2025-12-04 at 1 43 17 PM" src="https://github.com/user-attachments/assets/539554d8-bf98-4fce-8409-2ff5ea83c4a6" />

I used `break *0x40095f` to set a breakpoint after the random number is chosen. The number is still stored in the EAX register at this point, so I just printed it out from there.
<img width="817" height="526" alt="Screenshot 2025-12-04 at 1 46 08 PM" src="https://github.com/user-attachments/assets/13b10242-0102-4e73-ad31-aa7f605d0431" />

This printed the correct number before the input was asked, thus allowing me to get the flag. 

