# PicoCTF - General Skills: Password Cracking & Hashing
**Category:** General Skills  
**Difficulty:** Medium  
**Tags:** `password_cracking` `hashing`  

---

## Overview

Two challenges involving MD5 password hashing. The goal was to crack a hashed password and use it to decrypt an encrypted flag.

---

## Challenge 4 — Hardcoded Candidates

### Problem
- A Python password checker script was provided along with an encrypted flag and a hash file
- 100 candidate passwords were hardcoded inside the script
- Only 1 password, when MD5-hashed, would match the stored hash

### Approach

**Step 1:** Inspect the checker script to find the password list and understand the logic
```bash
cat level4.py
```

**Step 2:** Read the target hash from the binary file
```bash
xxd -p level4.hash.bin
# Output: d3d58c4786a6a229427351500ac7abd7
```

**Step 3:** Loop through all 100 candidates and compare MD5 hashes using bash
```bash
for p in "158f" "1655" "d21e" ... "d0e5"; do
  hash=$(echo -n "$p" | md5sum | cut -d' ' -f1)
  if [ "$hash" = "d3d58c4786a6a229427351500ac7abd7" ]; then
    echo "FOUND: $p"
  fi
done
```

**Step 4:** Use the found password in the checker script to decrypt the flag
```bash
python3 level4.py
# Enter the cracked password when prompted
```

### Concepts Learned
- `echo -n` — print without newline (important for correct hashing)
- `|` (pipe) — pass output of one command as input to another
- `md5sum` — generate MD5 hash of input
- MD5 is a one-way hash — you can't reverse it, but you can brute-force a small known list

---

## Challenge 5 — Dictionary Attack

### Problem
- Same structure as Level 4 but passwords were **not hardcoded**
- A separate `dictionary.txt` file was provided with many candidate passwords
- Required reading and iterating through the file in Python

### Approach

**Step 1:** Download all required files — checker script, encrypted flag, hash file, dictionary

**Step 2:** Write a Python script to read the dictionary and find the matching password

```python
import hashlib

# Read the correct hash from binary file
correct_hash = open("level5.hash.bin", "rb").read().hex()

# Try every password in the dictionary
with open("dictionary.txt", "r") as f:
    for line in f:
        password = line.strip()  # remove newline characters
        hashed = hashlib.md5(password.encode()).hexdigest()
        if hashed == correct_hash:
            print("FOUND PASSWORD:", password)
            break
```

**Step 3:** Run it in the PicoCTF web shell
```bash
python3 crack.py
```

**Step 4:** Enter the found password into the checker script to get the flag

### Concepts Learned
- `open(file, "rb")` — open file in binary read mode
- `open(file, "r")` — open file in text read mode
- `with` statement — safely opens and auto-closes files
- `.strip()` — removes whitespace/newlines from strings (crucial for correct hashing)
- `.encode()` — converts string to bytes for hashing
- `hashlib.md5().hexdigest()` — generates a readable MD5 hash string
- Dictionary attack — trying a known list of words against a hash

---

## Key Takeaways

| Concept | What it does |
|---|---|
| MD5 Hash | Converts any input into a fixed 32-char fingerprint |
| Pipe `\|` | Chains commands — output of left becomes input of right |
| `echo -n` | Prints text without a trailing newline |
| `.strip()` | Removes `\n` from file lines before hashing |
| Brute force | Try all possibilities from a known list |
| Dictionary attack | Same idea but using a wordlist file |

---

## Tools Used
- PicoCTF Web Shell
- Python 3 (`hashlib`)
- Bash (`md5sum`, `echo`, `for` loop)

---

*Solved as part of PicoCTF General Skills section — great intro to how password hashing and cracking works in the real world!*
