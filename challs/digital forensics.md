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

# Nutrela Chunks
One of my favorite foods is soya chunks. But as I was enjoying some Nutrela today, I noticed a few chunks weren’t quite right. Seems like something’s off with their structure. Could you help me fix these broken chunks so I can enjoy my meal again?

## Solution 
I first tried opening the file but it wasn't working. I tried looking into the file info but the original was a .png so i realised there was probably something wrong with the hex. 
I used [https://hexed.it/](url) to look into the .png and analysed the chunks and the header magic number to see if there was anything unusual. 

I also used ```pngcheck``` to see what was wrong with the file. 
<img width="482" height="50" alt="1" src="https://github.com/user-attachments/assets/308b6a44-b139-4ca0-8c00-72f9b9c34e7d" />
This was how I knew I had to further analyse the chunks. 

- First, I changed the magic number(header) to that of .png type ```89 50 4e 47 0d 0a 1a 0a```
  
<img width="442" height="43" alt="2" src="https://github.com/user-attachments/assets/7253ff07-fbf5-41c6-9821-681b75aedfa1" />

- Then I found that the IHDR chunk should be ```49 48 44 52```, not ```69 68 64 72```

  <img width="447" height="59" alt="3" src="https://github.com/user-attachments/assets/7d76a1e7-ca55-4374-98d5-335c72de0e87" />


- Then I found that the IDAT chunk should be ```49 44 41 54```, not ```69 64 61 74```

<img width="442" height="35" alt="4" src="https://github.com/user-attachments/assets/d26579b6-a1db-4fa9-bbb1-f8b308d6d165" />

- Then I found that the IEND chunk should be ```49 45 4E 44```, not ```69 65 6E 64```

  <img width="462" height="39" alt="5" src="https://github.com/user-attachments/assets/9b0edf26-2bad-4700-8181-66c8a071803c" />

- Finally the .png file could be opened from which I retrieved the flag

  <img width="944" height="945" alt="Screenshot 2025-11-10 at 12 20 04 AM" src="https://github.com/user-attachments/assets/4dc2ed1c-9e86-4020-b627-0151f8632ff3" />

## Flag:
```
nite{nOw_yOu_knOw_abOut_PNG_chunk5}
```

## Resources:
- [https://medium.com/@0xwan/png-structure-for-beginner-8363ce2a9f73](url)
- [https://en.wikipedia.org/wiki/PNG](url)

## Concpets learnt:
- Basically the whole reason the thing file was corrupt was because the chunk names were in lowercase when they should actually be in uppercase 

#  RAR of the Abyss
Two philosophers peer into the networked abyss and swap a secret. Use the secret to decrypt the Abyss’ RAwR and pull your flag from the void.

## Solution:

I first downloaded the file given and used Wireshark to analyze it. 
I opened the protocol hierarchy in statistics to see what all protocols were in the capture and saw this.

<img width="766" height="629" alt="Screenshot 2025-11-21 at 4 06 31 PM" src="https://github.com/user-attachments/assets/b4a9977b-7dab-40b4-8cb3-437540599ad5" />

This showed that TCP had a larger percentage of packages, so I researched more about TCP and then applied `tcp` as a filter to see what would show up. There were quite a few results, so I needed to reduce the number of packets I had to search through.

I used `tcp.len > 0` to get rid of the empty packets, and got 6 results from which only one seemed like it could be a rawr file. 
<img width="589" height="531" alt="Screenshot 2025-11-21 at 4 14 57 PM" src="https://github.com/user-attachments/assets/ccce0d0e-83f3-43f2-986a-c81c183a1d1f" />

I also got a password from another one of the packets, which was `b3y0ndG00dand3vil`.
<img width="594" height="538" alt="Screenshot 2025-11-21 at 4 16 34 PM" src="https://github.com/user-attachments/assets/8bc52c7a-87a0-4f61-975b-81a947a445fb" />

I then saved the rawr file by following the TCP stream and then used `unar -p b3y0ndG00dand3vil abyss.rar` to get the flag. 
<img width="540" height="89" alt="Screenshot 2025-11-21 at 4 20 32 PM" src="https://github.com/user-attachments/assets/bfa27917-9a49-4a3c-b1f1-916415723311" />

## Flag:
```
nite{thus_sp0k3_th3_n3tw0rk_f0r3ns1cs_4n4lyst}
```

## Resources: 
- [https://www.mankier.com/1/unar](url)
- [https://www.wireshark.org/docs/wsug_html_chunked/ChWorkBuildDisplayFilterSection.html](url)
- [https://www.geeksforgeeks.org/ethical-hacking/tcp-analysis-using-wireshark/](url)
- [https://techcommunity.microsoft.com/discussions/windowsinsiderprogram/how-do-i-easily-extract-rar-files-on-mac/4389894](url)

## Concepts learnt:
- I learnt about TCP and how you can use `unar` to extract info from files like RAR files, ZIP files, etc.

# NineTails 
Looks like I got a little too clever and hid the flag as a password in Firefox, tucked away like one of NineTails’ many tails. Recover the "logins" and the "key4" and let it guide you to the flag.

Hint:
I named my Ninetails "j4gjesg4", quite a peculiar name, isn't it?

## Solution: 
