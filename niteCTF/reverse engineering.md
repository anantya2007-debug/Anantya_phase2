# Lament of the Seraph

## Description 
Our friendly neighbourhood crypto bro is left lamenting after a ransomware attack wiped his fortune and locked his wallets. What he thought was a gateway to wealth was, in truth, a trap. Among his many missteps, some say he once placed his trust in a public server. Help him undo his folly and seize the flag in the process.
Note: This is a live malware, please exercise caution.
Drive Link: seraphslance.zip

## Solution 

Using rabin2 for static analysis of seraphs_lance.exe
<img width="530" height="201" alt="Screenshot 2025-12-16 at 9 55 11 PM" src="https://github.com/user-attachments/assets/2368b64e-7457-4924-87de-e91760c195e1" />

The presence of .tlc but no tlc callbacks suggested the precense of manual control-flow. 

I then used radare2 to map the functions and calls. 
<img width="576" height="273" alt="Screenshot 2025-12-16 at 10 12 33 PM" src="https://github.com/user-attachments/assets/233dfded-f162-4081-b2cb-e67297939ba8" />

<img width="717" height="425" alt="Screenshot 2025-12-16 at 10 12 14 PM" src="https://github.com/user-attachments/assets/6295c062-8daa-4d3e-96b6-8cf7db4be7ce" />
