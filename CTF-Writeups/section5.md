# picoCTF - Section 5 Writeup

**Platform:** picoCTF  
**Section:** 5 (Beginner's Guide to the Challenge Library)  
**Challenges Completed:** 5/5  
**Categories:** Forensics, Reverse Engineering, Binary Exploitation  

---

## 1. Enhance!

**Category:** Forensics  
**Difficulty:** Easy

### Description
Given an image file, the goal is to find the hidden flag inside it.

### Approach
- Opened the file and examined its contents
- Used inspection tools to look inside the image data
- The flag was embedded within the file's raw content

### Key Concept
Files can contain hidden data beyond what is visible. Always examine raw file contents in forensics challenges.

### Flag
`picoCTF{...}`

---

## 2. Big Zip

**Category:** Forensics  
**Difficulty:** Easy

### Description
A large zip file is provided containing many files and folders. The flag is hidden somewhere inside.

### Approach
- Unzipped the archive using `unzip bigzip.zip`
- Used `grep` to search all files at once:
```bash
grep -r "picoCTF{" .
```
- The flag was found inside one of the nested files

### Key Concept
`grep -r` recursively searches through all files in a directory — essential for finding flags in large archives.

### Flag
`picoCTF{...}`

---

## 3. vault-door-training

**Category:** Reverse Engineering  
**Difficulty:** Easy

### Description
A Java program acts as a "vault door" that checks if your input password is correct. The goal is to find the correct password.

### Approach
- Read the Java source code provided
- Found the password hardcoded in a `checkPassword()` function
- The function directly compared input against a plaintext string

### Key Concept
Always read source code carefully in reverse engineering. Hardcoded credentials are a common vulnerability.

### Flag
`picoCTF{...}`

---

## 4. keygenme-py

**Category:** Reverse Engineering  
**Difficulty:** Medium

### Description
A Python "keygen" program validates a license key. The goal is to reverse engineer the key generation logic and produce a valid key.

### Approach
- Read the Python source code
- Identified the key structure:
  - **Static part 1:** `picoCTF{1n_7h3_kk3y_of_`
  - **Dynamic part:** 8 characters derived from SHA-256 hash of username `BENNETT`
  - **Static part 2:** `}`
- The dynamic part was built by picking specific indices from the hash in scrambled order: `[4, 5, 3, 6, 2, 7, 1, 8]`
- Wrote a Python script to compute the key:

```python
import hashlib

h = hashlib.sha256(b"BENNETT").hexdigest()
dynamic = h[4] + h[5] + h[3] + h[6] + h[2] + h[7] + h[1] + h[8]
flag = "picoCTF{1n_7h3_kk3y_of_" + dynamic + "}"
print(flag)
```

### Key Concept
`hashlib.sha256()` produces a deterministic 64-character hex digest. Reverse engineering key generation logic lets you forge valid keys.

### Flag
`picoCTF{1n_7h3_kk3y_of_08c46aa4}`

---

## 5. buffer overflow 0

**Category:** Binary Exploitation  
**Difficulty:** Medium

### Description
A C program with a vulnerable buffer. The goal is to trigger a segmentation fault to print the flag.

### Approach
- Read the C source code (`vuln.c`)
- Identified the vulnerability:
  - `buf2[16]` — only 16 bytes
  - `strcpy(buf2, input)` — copies with no bounds check
- A SIGSEGV handler was set up to print the flag on crash
- Sent 100 characters to overflow the buffer:

```bash
python3 -c "print('A' * 100)" | nc <host> <port>
```

### Key Concept
Buffer overflows occur when more data is written into a buffer than it can hold. `strcpy()` and `gets()` are unsafe functions that do not check input size. Overflowing the stack corrupts the return address and causes a crash (SIGSEGV).

### Flag
`picoCTF{...}`

---

## Summary

| Challenge | Category | Key Tool/Concept |
|---|---|---|
| Enhance! | Forensics | File inspection |
| Big Zip | Forensics | `grep -r` |
| vault-door-training | Reverse Engineering | Hardcoded credentials in Java |
| keygenme-py | Reverse Engineering | SHA-256, key generation logic |
| buffer overflow 0 | Binary Exploitation | Stack buffer overflow, SIGSEGV |

---

*Completed as part of picoCTF Beginner's Guide to the Challenge Library — Section 5*
