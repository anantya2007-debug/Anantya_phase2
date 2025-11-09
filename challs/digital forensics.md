# Hide and seek 
Sakamoto’s at it again with a game of hide and seek, but this time, it’s not with Shin or his daughter. An old friend hid some secret data in this image. Can you find it before the others do?

Hint:
Even in retirement, Sakamoto never loses at hide and seek. Maybe stegseek can help you keep up.

## Solution:
I first installed steghide and stegseek using my terminal. After downloading the ```sakamoto.jpg``` file, I used the following command to see if I could directly extract the text from it wihtout a specific passphrase. 
<img width="516" height="100" alt="Screenshot 2025-11-09 at 11 17 37 PM" src="https://github.com/user-attachments/assets/824025c8-d13e-4ab8-9b30-eb6584bcde38" />

I then used this command to get the passphrase and save the found text in a file called ```cracked.txt```.

<img width="713" height="89" alt="Screenshot 2025-11-09 at 11 29 40 PM" src="https://github.com/user-attachments/assets/c25a94f3-a09f-41e8-8c24-ad5995e8d1f9" />

I then ```cat``` the file to retrieve the flag.
<img width="390" height="30" alt="Screenshot 2025-11-09 at 11 31 13 PM" src="https://github.com/user-attachments/assets/b98add69-97dc-4339-8020-6fd7b69811ad" />

## Flag:
```
nite{h1d3_4nd_s33k_but_w1th_st3g_sdfu9s8}
```
## Resources:
- [https://www.youtube.com/watch?v=L_dKHVxerVo](url)

## Concpets learnt:
- Stegseek uses a ```rockyou.txt``` wordlist if no other wordlist is specified (which was the case here)

