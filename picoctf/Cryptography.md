# 1. rsa_oracle 

Can you abuse the oracle?
An attacker was able to intercept communications between a bank and a fintech company. They managed to get the message (ciphertext) and the password that was used to encrypt the message.
Additional details will be available after launching your challenge instance.

After some intensive reconnaissance, they found out that the bank has an oracle that was used to encrypt the password and can be found here ```nc titan.picoctf.net 61563```. Decrypt the password and use it to decrypt the message. The oracle can decrypt anything except the password.

## Solution

## Flag:
```

```

## Concepts learnt:


## Resources:

# 2. Custom encryption

Can you get a sense of this code file and write the function that will decode the given encrypted file content.
Find the encrypted file here flag_info and code file might be good to analyze and get the flag.

## Solution

By analyzing the given encrytion code i tried reversing what had been done to decrypt the code. 
The encryption consisted of 
1. Shared lock (Diffie-Hellman formula)
2. A multiplication lock
3. XOR + reversal lock

### Shared lock (Diffie-Hellman formula)
This is essentially creating two keys for a and b in such a way that they open the same lock (i.e. Diffie-Hellman formula)
Here, by using the public numbers ```g``` and ```p```, the private numbers ```a``` and ```b``` are connected to make a secret common key which in this case is 83.
<img width="677" height="161" alt="Screenshot 2025-10-27 at 9 05 45 PM" src="https://github.com/user-attachments/assets/bc8559bb-08a4-4877-81fd-06134b72e137" />


```bash
 def decrypt(cipher, key):
    plaintext = ""
    for num in cipher:
        # Reverse the multiplicative encryption: cipher = (char * key * 311)
        if num == 0:
            plaintext += chr(0)
        else:
            char_val = num // (key * 311)
            plaintext += chr(char_val)
    return plaintext

def dynamic_xor_decrypt(cipher_text, text_key):
    plain_text = ""
    key_length = len(text_key)
    for i, char in enumerate(cipher_text):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        plain_text += decrypted_char
    return plain_text[::-1]  # Reverse at the end since encryption was done in reverse

def generator(g, x, p):
    return pow(g, x) % p

def decode_flag(cipher):
    p = 97
    g = 31
    a = 88
    b = 26
    
    # Regenerate the shared key
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    
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

