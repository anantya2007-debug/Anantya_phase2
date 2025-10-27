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

On opening the given Python code, I realized that a few lines of code had to be changed. 

I first replaced the values of a and b with the given values. 
<img width="495" height="162" alt="Screenshot 2025-10-27 at 7 27 34 PM" src="https://github.com/user-attachments/assets/b4f15629-5a53-41fa-984c-e0aee985e8c2" />

I then changed the code for the ```is_prime()``` function so it would actually return the correct boolean value if the input argument is a prime number or not. 
<img width="337" height="127" alt="Screenshot 2025-10-27 at 7 28 47 PM" src="https://github.com/user-attachments/assets/874fabf7-1fed-403e-bfdb-d763d4493390" />


