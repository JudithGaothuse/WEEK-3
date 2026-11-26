# Week 3: Password Cracking with John the Ripper & Networkwalks Tools

My Week 3 project for the Networkwalks Academy cybersecurity course. I cracked the passwords of locked PDF files using two different methods: John the Ripper (JTR) in Kali Linux, and Networkwalks' own online tools.

> Built as part of the Networkwalks Academy cybersecurity course (Week 3, Project Modules 1 and 2).

## Why this matters

When a file is locked with a password, the password isn't stored directly. It's turned into a hash, a scrambled value. To find the real password, you pull out that hash and run it against a list of possible passwords until one matches. This shows why short, common passwords are unsafe: they get cracked in seconds.

## Part 1: Password Cracking with John the Ripper (W3-PM1)

John the Ripper comes built into Kali Linux, so no download or Windows tools were needed.

### Step 1: Get the hash out of the PDF

```bash
mkdir -p ~/week3 && cd ~/week3
pdf2john "My Locked PDF1.pdf" > hash1.txt
cat hash1.txt
```

`pdf2john` reads the locked PDF and writes its crackable hash (starts with `$pdf$...`) into a text file.

### Step 2: Run John against the hash

```bash
clear; date
john hash1.txt
```

John tries its own default password list first (`/usr/share/john/password.lst`).

### Step 3: If it's not cracked yet, try a bigger wordlist

```bash
sudo gzip -d /usr/share/wordlists/rockyou.txt.gz 2>/dev/null
john --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt
```

(I didn't need this step. John's default list cracked every file.)

### Step 4: Show the cracked password

```bash
john --show hash1.txt
```

### Step 5: Open the PDF with the password found

```bash
xdg-open "My Locked PDF1.pdf"
```

I repeated steps 1, 2 and 4 for each locked PDF, just changing the file name and hash file name each time (`hash2.txt`, `hash3.txt`).

### My results

| File | Command | Password found |
|---|---|---|
| My Locked PDF1.pdf | `pdf2john` + `john` | `good-luck` |
| My Locked PDF2.pdf | `pdf2john` + `john` | `password1` |
| My Locked PDF3.pdf | `pdf2john` + `john` | `1qaz2wsx` |

All three cracked using John's own default password list. No bigger wordlist was needed.

### Flags captured

- PDF1: `nw{cybersecurity_flag_captured_2608}`
- PDF2: `nw{networkwalks_persistence_jtr_270521}`
- PDF3: `nw{networkwalks_flag_260821_1}`

## Part 2: Password Cracking with Networkwalks Tools (W3-PM2)

This part used two free tools made by Networkwalks that run entirely in the browser, no installation needed.

### Step 1: Extract the hash

1. Open `https://networkwalks.com/hash-calculator/`
2. Click the **PDF** tab
3. Upload the locked PDF
4. Copy the full hash shown (starts with `$pdf$`)

### Step 2: Crack the hash

1. Open `https://networkwalks.com/password-cracker/`
2. Paste the hash into the box
3. Click **Start Cracking**
4. Wait for the tool to try its built-in wordlist until it finds a match

### Step 3: Open the PDF

Enter the password the tool found to unlock the file.

### My result

| File | Tool | Password found |
|---|---|---|
| My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf | Networkwalks Hash Calculator + Password Cracker | `password1` |

Flag captured: `nw{networkwalks_flag1_jtr_270521_1}`

## What I learned

- A locked file's password is stored as a hash, not as plain text. Cracking means testing passwords until one produces a matching hash.
- John the Ripper and the Networkwalks Password Cracker both work the same basic way: try words from a list against the hash.
- Weak, common passwords (like `password1` or `good-luck`) are cracked in seconds. This is why strong, random passwords matter.
- Kali Linux comes with John the Ripper built in, so no extra download was needed for this task.
- Online cracking tools are convenient but should never be used on real, sensitive files. This lab only used safe practice files made for learning.

## Disclaimer

This work is for learning only. All files cracked here were provided by Networkwalks Academy for practice. Never use these methods on files or accounts you don't own or don't have permission to test.

## Credits

Lab requirements from Networkwalks Academy's cybersecurity course. Commands, screenshots and write-up are my own.
