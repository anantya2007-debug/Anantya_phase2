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
<img width="435" height="120" alt="Screenshot 2025-10-24 at 8 16 23 PM" src="https://github.com/user-attachments/assets/3207e5e0-25c0-4cc1-9e6e-c3541d18d1b1" />

In order to cause a buffer overflow, I had to input something that had a length of more than 16, thus causing ```sigsegv_handler()``` to print the flag. 

<img width="435" height="44" alt="Screenshot 2025-10-24 at 8 21 05 PM" src="https://github.com/user-attachments/assets/0ec57490-dc8a-4000-b972-5422e79f2f60" />

## Flag:
```
picoCTF{ov3rfl0ws_ar3nt_that_bad_9f2364bc}
```

## Concepts learnt:

- A **buffer overflow** is a software vulnerability that occurs when a program attempts to write more data into a fixed-size block of memory, or "buffer," than it was designed to hold. This extra data spills over into adjacent memory locations, overwriting and corrupting other data. 

## Resources:

- [https://www.imperva.com/learn/application-security/buffer-overflow/](url)

# 2. Format string 0

Can you use your knowledge of format strings to make the customers happy?
Download the binary here.
Download the source here.
Additional details will be available after launching your challenge instance.

Connect with the challenge instance here:
```nc mimas.picoctf.net 65431```

## Solution

The Format String exploit occurs when the submitted data of an input string is evaluated as a command by the application. 
In the line of code below, the format specifier is not mentioned (i.e. ```%s```) so I thought that this could possibly trigger some vulnerabilities. 
<img width="230" height="111" alt="Screenshot 2025-10-25 at 9 04 15 AM" src="https://github.com/user-attachments/assets/996d094b-ad54-430a-a21e-0c81393a2c6b" />

For the first input, I tried a random option first, but it returned this. 
<img width="563" height="133" alt="Screenshot 2025-10-25 at 9 06 25 AM" src="https://github.com/user-attachments/assets/d586b85f-88c2-42c6-87b9-6150db572ab7" />

This seemed like it was hinting at something with a larger length. 
The format specifier ```%114d``` was seen in ```Gr%114d_Cheese```. This is a string exploit used to control the output width. 

In order to get the flag, I needed a second choice that includes a format specifier. 
The only such option was ```Cla%sic_Che%s%steak```.

After entering this, the flag was printed. 
<img width="910" height="220" alt="Screenshot 2025-10-25 at 9 03 57 AM" src="https://github.com/user-attachments/assets/d8cf02a6-9272-42d0-a903-75e046421955" />

## Flag:
```
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_dc0f36c4}
```

## Concepts learnt:

- The **Format String exploit** occurs when the submitted data of an input string is evaluated as a command by the application.
- ```%d``` is a format specifier which gives a decimal integer. When a number is placed in front of it, the number is how many decimal numbers the program will give during execution 

## Resources:

- [https://owasp.org/www-community/attacks/Format_string_attack](url)


# 3. Clutter-overflow 

Clutter, clutter everywhere, and not a byte to use.
```nc mars.picoctf.net 31890```

## Solution 

## Concepts learned 

- GBD is a tool used for debugging programs by letting you examine their internal state while the program is running 

