- You can spot impersonation attempts by looking to see if the sender's email matches the internal domain or the standard email structure of the company
- Social engineering in phishing
- **Typosquatting** is when an attacker registers a common misspelling of an organisation's domain
- **punycode** is a special encoding system that converts Unicode characters (used in writing systems like Chinese, Cyrillic, and Arabic) into ASCII
- **spoofing** is a way attackers can trick users into thinking they are receiving emails from a legitimate domain.

- if an email really comes from who it says it does:
    1. SPF: Says which servers are allowed to send emails for a domain (like a list of approved senders).
    2.  DKIM: Adds a digital signature to prove the message wasn’t changed and really came from that domain.
    3.  DMARC: Uses SPF and DKIM to decide what to do if something looks fake (for example, send it to spam or block it).
