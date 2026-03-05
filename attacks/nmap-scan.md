# 🔍 Nmap Scan Results

## Target
```
Host: 192.168.1.103 (Metasploitable 2)
Date: 2026-03-05
Tool: Nmap 7.95
```

---

## Command
```bash
nmap -sV 192.168.1.103
```

---

## Results

```
Starting Nmap 7.95 at 2026-03-05 10:10 EST
Nmap scan report for 192.168.1.103
Host is up (0.00024s latency).
Not shown: 977 closed tcp ports (reset)

PORT     STATE  SERVICE      VERSION
21/tcp   open   ftp          vsftpd 2.3.4
22/tcp   open   ssh          OpenSSH 4.7p1 Debian 8ubuntu1
23/tcp   open   telnet       Linux telnetd
25/tcp   open   smtp         Postfix smtpd
53/tcp   open   domain       ISC BIND 9.4.2
80/tcp   open   http         Apache 2.2.8 (Ubuntu) DAV/2
111/tcp  open   rpcbind      2 (RPC #100000)
139/tcp  open   netbios-ssn  Samba smbd 3.X-4.X
445/tcp  open   netbios-ssn  Samba smbd 3.X-4.X
512/tcp  open   exec         netkit-rsh rexecd
513/tcp  open   login
514/tcp  open   tcpwrapped
1099/tcp open   java-rmi     GNU Classpath grmiregistry
1524/tcp open   bindshell    Metasploitable root shell
2049/tcp open   nfs          2-4 (RPC #100003)
2121/tcp open   ftp          ProFTPD 1.3.1
3306/tcp open   mysql        MySQL 5.0.51a-3ubuntu5
5432/tcp open   postgresql   PostgreSQL DB 8.3.0-8.3.7
5900/tcp open   vnc          VNC (protocol 3.3)
6000/tcp open   X11          (access denied)
6667/tcp open   irc          UnrealIRCd
8009/tcp open   ajp13        Apache Jserv (Protocol v1.3)
8180/tcp open   http         Apache Tomcat/Coyote JSP 1.1

MAC Address: 00:0C:29:EA:D2:85 (VMware)
Service Info: Hosts: metasploitable.localdomain, irc.Metasploitable.LAN
```

---

## Key Vulnerabilities Identified

| Port | Service | Version | Known Vulnerability |
|------|---------|---------|-------------------|
| 21 | FTP | vsftpd 2.3.4 | ⚠️ Backdoor (CVE-2011-2523) |
| 22 | SSH | OpenSSH 4.7p1 | ⚠️ Outdated version |
| 80 | HTTP | Apache 2.2.8 | ⚠️ EOL, multiple CVEs |
| 1524 | bindshell | Metasploitable root shell | ⚠️ Direct root access |
| 3306 | MySQL | 5.0.51a | ⚠️ Outdated, no auth |
| 5900 | VNC | protocol 3.3 | ⚠️ Weak authentication |
| 6667 | IRC | UnrealIRCd | ⚠️ Backdoor (CVE-2010-2075) |

---

## Suricata Response
Nmap scan triggered multiple Suricata alerts for protocol anomalies and port scanning behavior across HTTP, SMB, FTP, PostgreSQL and VNC services.
