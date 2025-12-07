#Joy Division 

## Solution: 
I first checked the file types of the given files. 
<img width="1508" height="109" alt="Screenshot 2025-12-06 at 10 50 14 AM" src="https://github.com/user-attachments/assets/f463e2c5-93a9-4623-b067-cf9ee36ef5b7" />

I ran the `disorder` file and got the following output. 
<img width="730" height="208" alt="Screenshot 2025-12-06 at 10 55 57 AM" src="https://github.com/user-attachments/assets/799725cd-7096-42e1-bee0-feef1e5c9979" />

From this, I could see that it was trying to open a file, which I'm assuming is `flag.txt`, but the file wasn't being opened correctly. 

I then backtraced to see the stack at the moment of the crash. It showed that the file pointer was set to null, which probably meant that the fopen() function had failed. 
<img width="714" height="102" alt="Screenshot 2025-12-06 at 11 01 22 AM" src="https://github.com/user-attachments/assets/5d9e4857-7249-4afd-aa5a-35cebc0db4bf" />

I then disassembled the main to look more into what was happening with the file. This is where the fopen() is opening a file.  
<img width="735" height="104" alt="Screenshot 2025-12-06 at 11 07 49 AM" src="https://github.com/user-attachments/assets/c2257f57-b7e3-46da-83d4-9438d173836e" />

Further, I saw that the file that was opened was not the same file that we needed for the flag.

<img width="444" height="68" alt="Screenshot 2025-12-06 at 11 08 28 AM" src="https://github.com/user-attachments/assets/d22ba794-dd4b-4725-a96f-835389735fe6" />
I tried renaming my file to the file mentioned above, but the result still looked like garbage, even tho program was now running fully. 

Now, when running the program in gdb, the output has changed. 
<img width="737" height="203" alt="Screenshot 2025-12-06 at 11 21 14 AM" src="https://github.com/user-attachments/assets/68ce61c2-5f2f-47ac-bc6e-48bdaae12c9f" />

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

# VeridisQuo

## Solution: 
I first ran `file VeridisQuo.apk` to see what the file type is, after which I unzipped the file.
<img width="806" height="123" alt="Screenshot 2025-12-04 at 2 28 08 PM" src="https://github.com/user-attachments/assets/ae5e5dc1-47bb-45e9-87e5-0b4f534b944f" />

I then created a folder and moved the APK into it. I then decompiled using the command `apktool d VeridisQuo.apk -o VeridisQuo_src`

<img width="1092" height="482" alt="Screenshot 2025-12-04 at 4 50 58 PM" src="https://github.com/user-attachments/assets/1f957f30-67a9-4d2c-93d3-6a9c401e4592" />

`smali` contains the decompiled code of the app, so I tried going further into that.

I then used `jadx-gui` to decompile the APK into readable Java code. I saw that there were mentions of a flag in `utilities`, and I understood from it that the flag would be 28 characters long. 

<img width="665" height="626" alt="Screenshot 2025-12-04 at 5 31 54 PM" src="https://github.com/user-attachments/assets/7560a840-f366-4ba5-b879-e67df6a4f3a1" />

I went into `resources<res<layout<activity_main.xml` and found flag parts 1 through 28, with each having one character. The previous screenshot shows that the flag parts are cleared during runtime. 

<img width="692" height="622" alt="Screenshot 2025-12-04 at 6 09 43 PM" src="https://github.com/user-attachments/assets/c1d2cd5e-3a46-41ee-b662-4284df2ecac3" />

I then got the flag by plotting the characters. 

<img width="720" height="367" alt="Screenshot 2025-12-04 at 7 41 03 PM" src="https://github.com/user-attachments/assets/5d1dcd69-a645-40ba-ae5d-6f384b61afe7" />

## Flag:
```
byuctf{android_piece_0f_c4ke}
```

## Resources:
-[https://www.youtube.com/watch?v=oJl52fbGKlE](url)

# Dusty

## Solution:
## Dust_noob
<img width="727" height="113" alt="Screenshot 2025-12-07 at 3 25 44 PM" src="https://github.com/user-attachments/assets/82d9e7ac-4908-4ad4-a1a1-704e6009219e" />

<img width="750" height="84" alt="Screenshot 2025-12-07 at 3 25 56 PM" src="https://github.com/user-attachments/assets/42783a6e-c0a5-4813-b6d7-f11b80c74b15" />

On disassembling the main, I got the function body was in `_ZN10shinyclean4main17h4b15dd54e331d693E`

<img width="737" height="389" alt="Screenshot 2025-12-07 at 3 30 36 PM" src="https://github.com/user-attachments/assets/6ad989b8-4e3f-4496-820e-fc471ec41fc0" />

I then saw a long chain of movb instructions that was writing a single byte into an array. I wrote down this array and got 
```bash
0x7b
0x5e
0x48
0x58
0x7c
0x6b
0x79
0x44
0x79
0x6d
0x0c
0x0c
0x60
0x7c
0x0b
0x6d
0x60
0x68
0x0b
0x0a
0x77
0x1e
0x42
```
After this, I saw that the loop was using XOR with 0x3f to get the output. 

<img width="612" height="93" alt="Screenshot 2025-12-07 at 3 44 38 PM" src="https://github.com/user-attachments/assets/aa1663d2-6a85-4418-8fb8-5f250e71a290" />

From this, I got the flag. 

## Flag:
```
DawgCTF{FR33_C4R_W45@!}
```

## Dust_intermediate

<img width="748" height="150" alt="Screenshot 2025-12-07 at 3 48 27 PM" src="https://github.com/user-attachments/assets/a4d85b92-8d99-462f-a29f-63656f10757a" />

<img width="736" height="137" alt="Screenshot 2025-12-07 at 3 49 03 PM" src="https://github.com/user-attachments/assets/34ebe0dd-2d10-4b83-831d-97a1957e0667" />

It was similar to the last one, where the function body was in `_ZN11shinyclean24main17h38206fcee08f84d4E`. 

<img width="735" height="359" alt="Screenshot 2025-12-07 at 3 52 36 PM" src="https://github.com/user-attachments/assets/e40204de-b22f-42ec-90f3-8444c7cab77c" />

From this, I saw that the program was sending the input string into a worker thread through a channel, which then somehow becomes transformed and received back, which was then compared with the bytes of another table. I printed this table to see what had to be received.   
<img width="745" height="203" alt="Screenshot 2025-12-07 at 4 07 39 PM" src="https://github.com/user-attachments/assets/59cb9e71-39f5-4d08-8281-547a7dc6c5fc" />

I opened the file on Ghidra and opened `shinyclean2::main`, which was where the main code was. From there, I found the `read_line` call, which mentioned that the input was going to `local_1b8`



## Flag:
```
DawgCTF{S0000_CL43N!}
```

## Dust_pro
<img width="734" height="137" alt="Screenshot 2025-12-07 at 6 34 29 PM" src="https://github.com/user-attachments/assets/6dcbed0c-2e38-4a92-ac3f-1f0d459db365" />
<img width="673" height="109" alt="Screenshot 2025-12-07 at 6 34 55 PM" src="https://github.com/user-attachments/assets/dfbd7fdf-b181-47b1-87c7-53007437c375" />

I found that the main logic was in `shinyclean::main`, so I disassembled it so i could inspect it. 
<img width="572" height="254" alt="Screenshot 2025-12-07 at 6 42 01 PM" src="https://github.com/user-attachments/assets/27b2eccf-ab88-4dc6-9079-fce6e2fe58bb" />

Same as the last time, I got an array of bytes. 
`cf 09 1e b3 c8 3c 2f af bf 24 25 8b d9 3d 5c e3 d4 26 59 8b c8 5c 3b f5 f6`

I also found a `SHA-256 hash` in .rodata, which I dumped to get this. 
`61cd3bdb1272953e049b0185b12703f8f6454c7df95c38cc042423c13e05ee51`

So basically, the program decrypts the bytes and compares it to make sure that SHA-256 hash of the bytes equals the expected hash. From whwihc i got `code = 139 + (104<<8) + (105<<16) + (212<<24)`

I then put this into the program to get the flag. 
 
<img width="664" height="151" alt="Screenshot 2025-12-07 at 7 00 12 PM" src="https://github.com/user-attachments/assets/8c567c85-06a9-4b10-8252-036beb8de94c" />

## Flag:
```
DawgCTF{4LL_RU57_N0_C4R!}
```


