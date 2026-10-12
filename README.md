# ARPple

ARPple is a network security lab project that demonstrates how **ARP spoofing** can change the local traffic path on a shared network and how this can be combined with **plaintext HTTP inspection**, **DNS spoofing**, and **HTTP response injection** in a controlled Windows–Kali lab environment.

This repository contains the final project report with screenshots, commands, observations, and defense notes.

## Project Overview

The project uses:

- **Windows 11 VM** as the target host
- **Kali Linux VM** as the attacking and monitoring host
- **Bettercap** for ARP spoofing, DNS spoofing, and HTTP proxy injection
- **Wireshark** for packet inspection and protocol evidence

The lab demonstrates five main ideas:

1. **ARP spoofing** changes the gateway mapping on Windows so that traffic is redirected through Kali.
2. **Plaintext HTTP** exposes readable form values when traffic is not protected by TLS.
3. **DNS spoofing** changes the hostname-to-IP resolution for `lab.test`.
4. **HTTP injection** modifies an HTTP response and inserts JavaScript into the page.
5. **Static ARP defense** pins the legitimate gateway MAC address and resists repeated dynamic ARP poisoning.

## Repository Contents

- [`ARPple_Report.pdf`](./ARPple_Report.pdf) — full project report

## Key Demonstrations

### 1. ARP Spoofing
Bettercap forges ARP replies so that the Windows system maps the gateway IP address to Kali’s MAC address. This changes the local delivery path and causes gateway-bound traffic to pass through Kali.

### 2. Plaintext HTTP Inspection
A test login page hosted on Kali is accessed from Windows over HTTP. Wireshark captures the HTTP POST request and shows that the submitted values are readable in plaintext.

### 3. DNS Spoofing
Bettercap spoofs the DNS response for `lab.test` and resolves it to Kali’s IP address (`10.10.10.129`). `nslookup` and `curl` confirm that the request reaches the Kali-hosted page.

### 4. HTTP Injection
A separate HTTP test page is served on port `8000`. Bettercap’s HTTP proxy injects JavaScript into the response, and the browser displays the alert message:

> You have been pwned!

### 5. Static ARP Defense
Windows is configured with a permanent ARP entry for the legitimate gateway MAC address. When the spoofing attempt is repeated, the entry remains permanent and the forged dynamic mapping does not replace it.

## Main Security Takeaways

- **ARP spoofing** affects local Layer 2 delivery by changing MAC resolution.
- **DNS spoofing** affects name resolution by changing the IP returned for a hostname.
- **HTTP** without TLS can expose and allow modification of traffic contents.
- **HTTPS** protects application data with encryption and reduces the impact of simple traffic interception.
- **Static ARP entries**, **network segmentation**, **client isolation**, and **Dynamic ARP Inspection (DAI)** help reduce the risk of ARP-based attacks.

## Notes

- This project is performed in a **controlled lab environment** for educational and defensive analysis.
- The report uses **test systems**, **test pages**, and **fake credentials** only.

## Author

**Shekoufeh Neisarian**
