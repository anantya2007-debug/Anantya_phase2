# Quick Mistake

## Description
Our internal service recently reported abnormal and inconsistent behavior. It is suspected that our network might have been compromised. It is also suspected that the attacker may have used the admin telemetry to their advantage. A packet capture taken during the incident window has been provided. Figure out what has been compromised and what internal data the attacker gained access to.
Flag Format: nite{attacker_ip_attacker_CID_flag_part_2}

## Solution
I used Wireshark to analyze the provided chal.pcap file. 
First, to identify the attacker IP, I used the following filters. 
- `tcp.flags.syn == 1 && tcp.flags.ack == 0` : to show who initiated connections
- `http || ftp || smb || mysql || postgres` : to show packets that belong to those application protocols

From the first filter, I got the attacker ID as it was the same ID being repeated with very few exceptions. 
