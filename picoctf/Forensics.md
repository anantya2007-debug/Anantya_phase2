# 2. tunn3l v1s10n

We found this file. Recover the flag.
Hint: Weird that it won't display right...

## Solution 
I first viewed the file in hex format using [https://hexed.it/](url). On opening, it looked something like this. 
<img width="1512" height="982" alt="Screenshot 2025-10-27 at 5 46 55 PM" src="https://github.com/user-attachments/assets/345412cd-97d8-4fd4-aad0-9ec437f699f7" />

I searched up the hex file format and got this.
<img width="549" height="203" alt="Screenshot 2025-10-27 at 6 12 15 PM" src="https://github.com/user-attachments/assets/36e69fc4-291d-41ed-8574-27468839fb0d" />





## Resources:
- [https://en.wikipedia.org/wiki/BMP_file_format#Bitmap_file_header](url)

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
