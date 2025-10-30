# 1. rsa_oracle 

Can you abuse the oracle?
An attacker was able to intercept communications between a bank and a fintech company. They managed to get the message (ciphertext) and the password that was used to encrypt the message.
Additional details will be available after launching your challenge instance.

After some intensive reconnaissance, they found out that the bank has an oracle that was used to encrypt the password and can be found here ```nc titan.picoctf.net 61563```. Decrypt the password and use it to decrypt the message. The oracle can decrypt anything except the password.

## Solution:
I first tried to directly decrypt the password using the given oracle but obvisouly that didnt work so i realised that i had to indirectly get the oracle to decrypt my password. 
I used the below code to get the password.

```bash
from subprocess import run, PIPE  
  
# password that is encrypted by RSA
c = 4228273471152570993857755209040611143227336245190875847649142807501848960847851973658239485570030833999780269457000091948785164374915942471027917017922546
  
print("Phase 1: Get password\n")  
  
print(f"c = {c}\n")  
 
# Get message from user
m1 = input("Enter message (m1): ")  
m1_bytes = bytes(m1, "utf-8")  # converting a string into a sequence of bytes using UTF-8 encoding
m1_int = ord(m1_bytes) #returns int value of first byte 
  
print(f"Have the oracle encrypt this message (m1): {m1}\n")  
c1 = int(input("Enter ciphertext from oracle (c1 = E(m1)): "))  # encrypted message from oracle 
print("\n")

# The homomorphic property which is basically when the mathematical operations on ciphertexts correspond to operations on plaintexts
c2 = c * c1 # essentially just E(m) * E(m1) = E(m * m1)
print(f"Have the oracle decrypt this message (c2 = c * c1): {c2}\n") # will give c2 = E(m) * E(m1) = E(m * m1)

m2 = int(input("Enter decrypted ciphertext as HEX (m2 = D(c2): "), 16) # will give  m2 = D(c2) = D(E(m * m1)) = m * m1 but the m we get here will be the decrypted version 
print("\n")
 
m_int = m2 // m1_int  # to find password m we just use m = m2 / m1
m = m_int.to_bytes(len(str(m_int)), "big").decode("utf-8").lstrip("\x00") # converting numerical result back to text
print(f"Password (m = m2 / m1): {m}\n")  

```

After I found the key, I put the following command into my terminal as mentioned in the hints, and then got the flag.
<img width="695" height="68" alt="Screenshot 2025-10-29 at 10 01 42 PM" src="https://github.com/user-attachments/assets/eed1673b-a395-4fc5-a8b5-d86818b32552" />


## Flag:
```
picoCTF{su((3ss_(r@ck1ng_r3@_da099d93}
```

## Concepts learnt:
- RSA has homomorphic properties, which is basically when the mathematical operations on ciphertexts correspond to operations on plaintexts


## Resources:

# 2. Custom encryption

Can you get a sense of this code file and write the function that will decode the given encrypted file content.
Find the encrypted file here flag_info and code file might be good to analyze and get the flag.

## Solution:

By analyzing the given encrytion code i tried reversing what had been done to decrypt the code. 
The encryption consisted of 
1. Shared lock (Diffie-Hellman formula)
2. A multiplication lock
3. XOR + reversal lock

### Shared lock (Diffie-Hellman formula)
This is essentially creating two keys for a and b in such a way that they open the same lock (i.e. Diffie-Hellman formula)
Here, by using the public numbers ```g``` and ```p```, the private numbers ```a``` and ```b``` are connected to make a secret common key, which in this case is 83.
<img width="677" height="161" alt="Screenshot 2025-10-27 at 9 05 45 PM" src="https://github.com/user-attachments/assets/bc8559bb-08a4-4877-81fd-06134b72e137" />

### A multiplication lock
In the encoded code, the line is ```cipher.append(((ord(char) * key*311)))```. Thus, to reverse this, I used ```char_val = num // (key * 311)``` to undo the multiplication lock. 

### XOR + reversal lock
Since XOR is a reversible operation, I just applied the same thing again and then reversed the text again to get the flag.


```bash
 def decrypt(cipher, key):
    plaintext = ""
    for num in cipher:
        # Reverse the multiplicative encryption: cipher = (char * key * 311)
        if num == 0:
            plaintext += chr(0)
        else:
            char_val = num // (key * 311) # ecryption code was: cipher.append(((ord(char) * key*311))) thus, we divide to reverse
            plaintext += chr(char_val)
    return plaintext

def dynamic_xor_decrypt(cipher_text, text_key):
    plain_text = ""
    key_length = len(text_key)
    for i, char in enumerate(cipher_text):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        plain_text += decrypted_char
    return plain_text[::-1]  # Reverse at the end since encryption was reversed at the beginning 

def generator(g, x, p):
    return pow(g, x) % p

def decode_flag(cipher):
    p = 97
    g = 31
    a = 88
    b = 26
    
    # Regenerate the shared key
    u = generator(g, a, p) # (g^a)%p
    v = generator(g, b, p)
    key = generator(v, a, p) #which would give the same result as generator(u, b, p) i.e. 83
    
    # First decrypt the multiplicative cipher
    semi_cipher = decrypt(cipher, key)
    
    # Then decrypt the XOR cipher
    plaintext = dynamic_xor_decrypt(semi_cipher, "trudeau")
    
    return plaintext

# The cipher from the file
cipher = [97965, 185045, 740180, 946995, 1012305, 21770, 827260, 751065, 718410, 457170, 0, 903455, 228585, 54425, 740180, 0, 239470, 936110, 10885, 674870, 261240, 293895, 65310, 65310, 185045, 65310, 283010, 555135, 348320, 533365, 283010, 76195, 130620, 185045]

# Decode the flag
flag = decode_flag(cipher)
print(f"Decoded flag: {flag}")
```

## Flag:
```
picoCTF{custom_d2cr0pt6d_019c831c}
```

## Resources:
- [https://www.geeksforgeeks.org/computer-networks/implementation-diffie-hellman-algorithm/](url)
- [https://www.geeksforgeeks.org/dsa/xor-cipher/](url)

# 3. Mini RSA

Let's decrypt this: ciphertext? Something seems a bit small.

## Solution:

The given N was too big, e too small, and c too small, so in the general formula ```c^e mod N``` we can ignore ```N``` and replace ```e``` with 3.

From the given data, it is seen that the giant number is the output of the input flag cubed (after the flag is turned into a number), i.e., (flag)^3=giant number.

In order to reverse this, I had to cube root the given number.

```bash
c=2205316413931134031074603746928247799030155221252519872649649212867614751848436763801274360463406171277838056821437115883619169702963504606017565783537203207707757768473109845162808575425972525116337319108047893250549462147185741761825125 
x = 1 << (c.bit_length()//3)  # the cube root of a number with length n will have n/3 no. of digits         
while True:
   y = (2*x + c//(x*x)) // 3 # Newton's Method for iterative algorithm for finding roots
   if y >= x:
       m = x; break
   x = y
plaintext = m.to_bytes((m.bit_length()+7)//8, 'big') # algorithm to convert int to bytes
print(plaintext.decode()) # convert bytes to string

```

## Flag:
```
picoCTF{n33d_a_lArg3r_e_606ce004}
```

## Resources:
- [https://en.wikipedia.org/wiki/RSA_cryptosystem](url)
- [https://crypto.stackexchange.com/questions/33561/cube-root-attack-rsa-with-low-exponent](url)

