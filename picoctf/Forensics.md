# 2. tunn3l v1s10n

We found this file. Recover the flag.
Hint: Weird that it won't display right...

## Solution 
I first viewed the file in hex format using [https://hexed.it/](url). On opening, it looked something like this. 
<img width="1512" height="982" alt="Screenshot 2025-10-27 at 5 46 55 PM" src="https://github.com/user-attachments/assets/345412cd-97d8-4fd4-aad0-9ec437f699f7" />

I searched up the hex file format and got this.
<img width="549" height="203" alt="Screenshot 2025-10-27 at 6 12 15 PM" src="https://github.com/user-attachments/assets/36e69fc4-291d-41ed-8574-27468839fb0d" />

When I first tried opening the hex as a png file, it said there was an error, so I tried comparing it to the hex of an already existing picture. 
I realized then that I had to change the header to make it ```28 00 00 00``` after which, when downloading the picture, I got a fake flag. 

<img width="487" height="81" alt="Screenshot 2025-10-27 at 6 47 30 PM" src="https://github.com/user-attachments/assets/f09cd547-71b5-4f48-998a-6b41b90fc2b7" />

![tunn3l_v1s10n](https://github.com/user-attachments/assets/fdae12cc-7320-4c3a-9656-f832f7088b8a)

This seemed to be only a part of the picture, so I tried increasing the width and height of the picture. 
By changing the width, the picture got messed up, so I changed it back to the original. 
<img width="1131" height="332" alt="Screenshot 2025-10-27 at 6 51 13 PM" src="https://github.com/user-attachments/assets/f5ca0c48-796e-4dd1-89dd-7d2809122052" />

I then tried changing the height, which then gave me the flag. 

<img width="1086" height="405" alt="Screenshot 2025-10-27 at 6 55 26 PM" src="https://github.com/user-attachments/assets/1cdf82fe-4c70-452b-8953-b166aaa386cf" />

## Flag:
```
picoCTF{qu1t3_a_v13w_2020}
```

## Resources:
- [https://en.wikipedia.org/wiki/BMP_file_format#Bitmap_file_header](url)
- [https://stackoverflow.com/questions/71812494/how-should-bmp-header-look-like](url)

## Concepts learnt:
Byte 14-17: Info header size (always 40 i.e 28 00 00 00)

Byte 18-21: Image width

Byte 22-25: Image height

Byte 26-27: Color planes (always 1)

Byte 28-29: Bits per pixel

# 3. m00nwalk

Decode this message from the moon.

## Solution
From the hint "How did pictures from the moon landing get sent back to Earth?", I was able to find that the pictures from the moon landing were sent in the form of SSTV (slow-scan television) transmissions. 

Using an online SSTV decoder [https://sstv-decoder.mathieurenaud.fr/](url), I was able to get an image from the provided wav file.

<img width="673" height="497" alt="Screenshot 2025-10-27 at 1 20 29 PM" src="https://github.com/user-attachments/assets/2f0b2864-07b1-44d3-94f3-3a1b0edf22bd" />

## Flag:
```
picoCTF{beep_boop_im_in_space}
```

## Resources:
- [https://en.wikipedia.org/wiki/Apollo_11_missing_tapes](url)

## Concepts learnt: 
- how sstv detetors work 
