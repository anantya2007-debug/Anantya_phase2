# Quick Mistake

## Description
Our internal service recently reported abnormal and inconsistent behavior. It is suspected that our network might have been compromised. It is also suspected that the attacker may have used the admin telemetry to their advantage. A packet capture taken during the incident window has been provided. Figure out what has been compromised and what internal data the attacker gained access to.
Flag Format: nite{attacker_ip_attacker_CID_flag_part_2}

## Solution
I used Wireshark to analyze the provided chal.pcap file. 
First, to identify the attacker IP, I used the following filters. 

- `tcp.flags.syn == 1 && tcp.flags.ack == 0` : to show who initiated connections

From the first filter, I got the attacker IP as it was the same IP being repeated with very few exceptions. 
<img width="941" height="463" alt="Screenshot 2025-12-16 at 2 51 56 PM" src="https://github.com/user-attachments/assets/62daf13d-b361-43c3-af80-bc21f56c91c8" />

The repeating IP seemed to have a lot of traffic so I assumed that it wasn't the attacker. 

### Attacker IP: 192.0.2.66

After using the filter `http.request` I found `/telemetry` which was also in the descripiton of the challenge. 
<img width="941" height="482" alt="Screenshot 2025-12-16 at 6 07 07 PM" src="https://github.com/user-attachments/assets/6a035644-b505-462a-97cd-2d6735996f4c" />

I then followed the HTTP stream. 
<img width="689" height="513" alt="Screenshot 2025-12-16 at 6 07 37 PM" src="https://github.com/user-attachments/assets/64c2498e-453e-4b34-887c-228fa819089f" />

I then got the attacker's CID. 
<img width="962" height="305" alt="Screenshot 2025-12-20 at 10 29 27 PM" src="https://github.com/user-attachments/assets/ea365c59-3e49-421b-8af7-5fd8d9c1179e" />

### Attacker SCID: 2457ce19cb87e0eb

Using the filter `quic`, I found a JSON `telemetry_sslkeylog`. 
Then used `http3.request.uri contains "source"` to find `tar.gz`. I extracted it to get `.env` file which had `AES_FLAG_KEY=wEN64tLF1PtOglz3Oorl7su8_GQzmlU2jbFP70cFz7c=`. 

I then used the following code to decrypt the flag. 

```bash
from cryptography.fernet import Fernet

KEY = b"wEN64tLF1PtOglz3Oorl7su8_GQzmlU2jbFP70cFz7c="
TOKEN = b"gAAAAABpNXDCHUJ4YqH0Md2p6tzE303L8z5kPpPPWwYYrXUdiyW89eCaWWL1dbYU2JYj7SUvdwySW_egZDRF0fyFGxPua2KoFmd8upKP7cZv55jVp_SzItA="

print(Fernet(KEY).decrypt(TOKEN).decode())
```

## Flag
```
nite{192.0.2.66_2457ce19cb87e0eb_qu1c_d4t4gr4m_pwn3d}
```


# Ophelia's Truth 1

## Description 

A detective at Moscow PD, Department 19, receives a message asking him to check the forensic analysis portal for a DNA report. Attached to the message is a file containing a link to the portal. He opens the attachment, but initially, nothing seems to happen, so he overlooks it. Later, he realizes that a crucial file from an ongoing case has gone missing.
He has provided the forensic artifacts from his computer to you, his colleague at the cyber forensics department, to figure out what went wrong. Find:

The filename of the attachment; 
The ip from where the malware was executed; 
The CVE the attacker exploited.
Flag format: nite{file_name.ext_XXX.XXX.XXX.XXX_CVE-XXXX-XXXXX}

## Solution 

I ran `python3 vol.py -f ~/Downloads/ophelia.raw windows.info` and used volatility to identify the system profile. 
<img width="502" height="261" alt="Screenshot 2025-12-16 at 6 52 20 PM" src="https://github.com/user-attachments/assets/09fdc729-0528-4dc3-943a-a80f3377abfb" />

From this, I got that it was a Windows 10 OS and the TimeDateStamp as 2025-12-07 14:00 UTC. 

I then used  `python3 vol.py -f /Users/suresh/Downloads/ophelia.raw windows.filescan \
| grep -iE "dna|forensic|report|portal" \
| head -20
` to find the file that matched the challenge description. 
<img width="975" height="213" alt="Screenshot 2025-12-16 at 7 01 34 PM" src="https://github.com/user-attachments/assets/c2c15c99-3f9c-4932-ae9f-9e700282280c" />

### Filename: `dna_analysis_portal.url`

I then further examined and extracted `dna_analysis_portal.url`.
<img width="563" height="80" alt="Screenshot 2025-12-16 at 7 10 25 PM" src="https://github.com/user-attachments/assets/6ce4afd7-6378-4d50-8a19-487d325f9821" />

I got the offset to be `0xc201a703b260`. 
<img width="512" height="158" alt="Screenshot 2025-12-16 at 7 16 41 PM" src="https://github.com/user-attachments/assets/7d5d21ea-3f99-4660-81cb-144d6753363d" />

From this, I got the ip from where the malware was executed as `10.72.5.205`.

Using the information that was gathered and going through the website, https://www.cisa.gov/known-exploited-vulnerabilities-catalog?page=5, I found the CVE that matched this particular vulnerability. 

<img width="768" height="454" alt="Screenshot 2025-12-16 at 7 31 27 PM" src="https://github.com/user-attachments/assets/19bf8b19-810a-4f94-a39b-c36d2923ef75" />

Putting all of this together, I got the final flag. 

## Flag
```
nite{dna_analysis_portal.url_10.72.5.205_CVE-2025-33053}
```




