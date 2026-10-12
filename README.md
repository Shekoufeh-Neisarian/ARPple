# ARPple

**ARP spoofing, DNS redirection, plaintext HTTP analysis, and traffic-manipulation defenses.**

ARPple explores how forged ARP replies can change the local traffic path between a Windows host and its gateway. The project also examines what an unencrypted HTTP request reveals, how a spoofed DNS answer redirects a hostname, how HTTP content can be modified, and how a permanent ARP entry can protect a gateway mapping.

## Technologies & Tools

| Category | Technologies |
| --- | --- |
| Operating systems | Kali Linux, Windows 11 |
| Network analysis | Wireshark, Bettercap |
| Virtualization | VMware |
| Languages used in test pages and commands | Python 3, HTML, JavaScript, PowerShell, Bash |
| Networking & security concepts | ARP, DNS, HTTP, TCP/IP, man-in-the-middle (MITM), static ARP entries |

**Language note:** Python was used to host HTTP test pages; HTML and JavaScript were used for the test pages and injection demonstration. PowerShell and Bash were used for system configuration and verification. This is a network-security demonstration, not a standalone application written in these languages.

## Lab Environment

| Component | Role | Lab address |
| --- | --- | --- |
| Windows 11 VM | Target, browser, and ARP-table verification | `10.10.10.128` |
| Kali Linux VM | Bettercap, Wireshark, and test web server | `10.10.10.129` |
| Gateway | Legitimate default gateway | `10.10.10.2` |

Both virtual machines operated on the same isolated lab subnet (`10.10.10.0/24`).

## What the Project Demonstrates

### 1. ARP spoofing

Bettercap sends forged ARP replies associating the gateway IP (`10.10.10.2`) with Kali's MAC address. Wireshark records the ARP replies, and the Windows ARP table confirms that the gateway mapping changes. IP forwarding is enabled on Kali during the test.

### 2. Plaintext HTTP inspection

A test login page is served directly from Kali and accessed from Windows over HTTP. Wireshark displays the form fields submitted in an HTTP POST request. The page uses **fake credentials**; this direct HTTP request is a separate test and does not itself prove interception of a third-party session.

### 3. DNS spoofing

A spoofed DNS response resolves `lab.test` to `10.10.10.129`. The result is checked using `nslookup` and an HTTP request with `curl.exe`, which receives the test page from Kali.

### 4. HTTP JavaScript injection

A separate test page on port `8000` demonstrates modification of unencrypted HTTP content. The browser displays a JavaScript alert: **“You have been pwned!”**

### 5. Static ARP defense

A persistent Windows neighbor entry binds the gateway IP to its legitimate MAC address. During the repeated spoofing attempt, that mapping remains **Permanent** instead of being replaced by a forged dynamic entry.

## Key Findings

- **ARP spoofing** changes the MAC address used to reach a local IP.
- **DNS spoofing** changes the IP address returned for a hostname.
- **Unencrypted HTTP** can expose form data and permit response modification.
- **HTTPS/TLS** protects application content; changing an ARP or DNS mapping alone does not decrypt a valid HTTPS connection.
- **Static ARP mappings** can protect a specific entry. Network-wide protections include Dynamic ARP Inspection, client isolation, and network segmentation.

## Project Report

The report contains the network setup, commands, screenshots, observed results, infographic, and cleanup procedures.

**[Read the ARPple Project Report (PDF)](./ARPple_Report_FINAL.pdf)**

## Scope

The experiments use virtual machines, controlled test pages, and fake credentials in an authorized lab environment.

## Author

**Shekoufeh Neisarian**
