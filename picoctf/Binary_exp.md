# 1. buffer overflow 0

Let's start off simple, can you overflow the correct buffer? The program is available here. You can view source here.
Additional details will be available after launching your challenge instance.

Connect using:
```nc saturn.picoctf.net 62121```

## Solution 

The question mentions buffer overflow, which is essentially when you write more information into a buffer than it is designed to hold.
The code given is shown below.

<img width="1512" height="982" alt="Screenshot 2025-10-24 at 8 16 02 PM" src="https://github.com/user-attachments/assets/6949caa4-44c7-4e88-8c92-5f387b7a3150" />

From this, I saw that the buffer size was given as 16
<img width="179" height="71" alt="Screenshot 2025-10-24 at 8 16 23 PM" src="https://github.com/user-attachments/assets/3207e5e0-25c0-4cc1-9e6e-c3541d18d1b1" />

In order to cause a buffer overflow, I had to input something that had a length of more than 16

<img width="435" height="44" alt="Screenshot 2025-10-24 at 8 21 05 PM" src="https://github.com/user-attachments/assets/0ec57490-dc8a-4000-b972-5422e79f2f60" />

## Flag:
```
picoCTF{ov3rfl0ws_ar3nt_that_bad_9f2364bc}
```

## Concepts learnt:

- A **buffer overflow** is a software vulnerability that occurs when a program attempts to write more data into a fixed-size block of memory, or "buffer," than it was designed to hold. This extra data spills over into adjacent memory locations, overwriting and corrupting other data. 

## Resources:

- [https://www.imperva.com/learn/application-security/buffer-overflow/](url)
