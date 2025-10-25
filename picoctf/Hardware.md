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

