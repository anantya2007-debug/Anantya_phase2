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



