- Ways of guessing the password that protects a file
     1. Dictionary Attacks: Use a predefined list of potential passwords (AKA wordlist); useful in case of weak or common passwords
     2. Mask Attacks: Basically, a Brute-force attack, but limits gueasses to a specific format to make it faster
     3. Brute-force Attacks: systematically tries every possible combination of characters until the right one is found

- Common wordlists
     1. rockyou.txt
     2. common-passwords.txt

- You can use GPU-accelerated cracking when possible (in cases such as MD5, SHA-1, SHA-256, NTLM)
- Tools to Use (pick one based on file type)
  1. PDF: pdfcrack, john (via pdf2john)
  2. ZIP: fcrackzip, john (via zip2john)
  3. General: john (very flexible) and hashcat (GPU acceleration, more advanced)
