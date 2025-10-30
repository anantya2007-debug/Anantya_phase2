# 1. IQ Test

let your input x = 30478191278.

wrap your answer with nite{ } for the flag.

As an example, entering x = 34359738368 gives (y0, ..., y11), so the flag would be nite{010000000011}.
<img width="530" height="944" alt="Screenshot 2025-10-25 at 11 56 12 AM" src="https://github.com/user-attachments/assets/5b621858-c0c1-44e4-98e6-012335dbc2e5" />


## Solution

First, I converted the input x = 30478191278 into binary by dividing by 2. 
I got x = 011100011000101001000100101010101110 in the case of 36 digits. 

I then replaced the values of x0, x1, x2, ..., x35 with the digits of binary x (starting from the MSB) and got the following output.
![WhatsApp Image 2025-10-25 at 17 42 57](https://github.com/user-attachments/assets/71b995ea-9803-49e2-aa2c-72284b01f606)

THe flag is in the form of ```nite{}``` and the values of y0, y1..., y11 inside the brackets. 

## Flag:
```
nite{100010011000}
```

## Notes:
In order to figure out if the values of x0, x1..., x35, started from MSB or the LSB, I used the example question to match the flag given to the values of ```y``` that I was getting. 

![WhatsApp Image 2025-10-25 at 17 42 57 (1)](https://github.com/user-attachments/assets/f6996a80-7a60-4650-bb63-173a65ca3943)


# I like Logic 
I like logic and I like files, apparently, they have something in common. What should my next step be?

## Solution:
When I opened the file given by them, I saw that it was a ```.json``` so i used ```Logic2``` to analyse the files inside the json. I saved the file as .SAL and when i opened it i saw that only 'channel 3' out of the 5 channels had a square wave. 

I used the Async Serial analyzer and changed the baud to different standard values. Using trial and error I got the value to be 9600 and saw the following. 

<img width="1512" height="982" alt="Screenshot 2025-10-29 at 11 59 07 PM" src="https://github.com/user-attachments/assets/a752ebb2-eafb-469b-9647-93fa03300fd6" />

<img width="1512" height="982" alt="Screenshot 2025-10-29 at 11 58 52 PM" src="https://github.com/user-attachments/assets/1cf89904-a6bc-414a-aa48-dab82b3c40da" />

I searched for curly brackets and then found the flag. 

## Flag:
```
FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}
```

# 3. Bare Metal Alchemist 
My friend recommended me this anime but i think i've heard a wrong name.

## Solution:

I first had to install AVR and its tools.
I then used the command ```avr-objdump -m avr -D ./firmware.elf``` to disassemble the file. 
It gave me something like this. 
<img width="1512" height="982" alt="Screenshot 2025-10-30 at 6 53 30 PM" src="https://github.com/user-attachments/assets/b5c59356-a7c7-4587-8b3c-f6fa2eedb07c" />

I saw that ```eor``` was in the name of the registers, which indicated the XOR operation. 
<img width="421" height="72" alt="Screenshot 2025-10-30 at 6 58 20 PM" src="https://github.com/user-attachments/assets/da3e8ad7-80ba-4c1b-be36-a09d11db0c82" />

In the next line ```	cpi	r24, 0xA5``` it can be seen that they are comparing the register r24 to 0xA5 so i assumed the XOR key to be ```0xA5```. 

I took the bytes starting from position 0x68 till 0x92 and put them as a list in my XOR code below. 

```bash
encoded_data = [
    0xf1, 0xe3, 0xe6, 0xe6, 0xf1, 0xe3, 0xde, 0xf1, 0xcd, 0x94, 0xd6, 0xfa,
    0x94, 0xd6, 0xfa, 0xd6, 0xca, 0xc8, 0x96, 0xfa, 0xd6, 0x94, 0xc8, 0xd5,
    0xc9, 0x96, 0xfa, 0x91, 0xd7, 0xc1, 0xd0, 0x94, 0xcb, 0xca, 0xfa, 0xc3,
    0x94, 0xd7, 0xc8, 0xd2, 0x91, 0xd7, 0xc0, 0xd8
]

key = 0xA5

decoded = bytes([byte ^ key for byte in encoded_data])
print("Decoded data:", decoded)
```
This gave me the following output. 
```
Decoded data: b'TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}'
```
From which I got the flag. 

## Flag:
```
TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}
```

