# 🛡️ My CTF Solutions — Cyber Induction

Writeups and notes from the **Cyber Induction CTF**, documented as part of my cybersecurity learning journey.

> **Ethical use notice:** Every challenge documented here was solved on infrastructure provided for the CTF, within its rules. Nothing in this repository should be used against systems you do not own or do not have explicit written permission to test. See [Ethics & Rules](#-ethics--rules).

---

## 🎯 Objectives

- Build a clear, reproducible record of how I solve each challenge.
- Practise the core CTF categories: Linux, Web, Crypto, Forensics, Stego, OSINT.
- Learn to explain technical steps simply, a key professional skill.
- Create a portfolio that shows my method, not just the final answer.

## 🧰 Tools Used

Update this list as you actually use each tool.

| Area | Tools |
|------|-------|
| General | Kali Linux / Ubuntu, terminal, Python 3, CyberChef |
| Linux | `ls`, `cat`, `grep`, `find`, `file`, `strings`, `ssh`, `chmod` |
| Web | Browser DevTools, `curl`, Burp Suite Community |
| Cryptography | `base64`, `openssl`, CyberChef, Python |
| Forensics | `file`, `exiftool`, `binwalk`, `strings`, `xxd`, Wireshark |
| Steganography | `steghide`, `zsteg`, `exiftool`, `binwalk` |
| OSINT | Search engines, `whois`, `dig`, Wayback Machine, `exiftool` |

Command explanations are in [tools-and-commands.md](tools-and-commands.md).

## 📚 Skills Learned

Replace these with your own reflections after each challenge:

- [ ] Navigating and searching a Linux file system from the command line
- [ ] Inspecting web requests, cookies and source code
- [ ] Recognising common encodings and simple ciphers
- [ ] Identifying file types and hidden data in files
- [ ] Extracting hidden messages from images/audio
- [ ] Gathering public information responsibly (OSINT)
- [ ] Writing clear, structured technical documentation

## 🗂️ Repository Structure

```
my-ctf-solutions/
├── README.md
├── writeup-template.md
├── tools-and-commands.md
├── resources.md
├── .gitignore
└── challenges/
    ├── linux/
    ├── web-exploitation/
    ├── cryptography/
    ├── forensics/
    ├── steganography/
    └── osint/
```

Each category folder has its own `README.md` and an `images/` folder for screenshots.

## 📋 Challenge Index

Add one row per solved challenge. The table starts empty on purpose, so only real, completed work is listed.

| # | Challenge | Category | Difficulty | Status | Writeup |
|---|-----------|----------|------------|--------|---------|
| 1 | _(add your first challenge)_ | | | | |

**Status key:** ✅ Solved · 🔄 In progress · ❌ Unsolved

## ✍️ How I Write Up a Challenge

1. Copy [writeup-template.md](writeup-template.md) into the right `challenges/<category>/` folder.
2. Name it `challenge-name.md` (lowercase, hyphens).
3. Fill in only what I actually did and saw. No guessed results.
4. Save screenshots in that category's `images/` folder.
5. Add a row to the index above.

## ⚖️ Ethics & Rules

- Only attempt challenges I am **authorised** to attempt (the CTF platform/organisers).
- Stay inside the **defined scope**: no attacking the CTF infrastructure, other participants, or unrelated sites.
- Follow the CTF's rules on **flag sharing and public writeups**. If the event is still running or forbids publishing solutions, keep writeups private or redact flags until allowed.
- Never commit credentials, VPN files, private keys, or personal data.
- OSINT: use only information relevant to the challenge. Do not collect or publish real people's private information.

## 📄 License

Add a license of your choice (for example MIT) via GitHub's "Add file → Create new file → LICENSE" option.

---

*Maintained by: `<Anubhav nabriya>` · Cyber Induction CTF · Learning in public* 🚀









