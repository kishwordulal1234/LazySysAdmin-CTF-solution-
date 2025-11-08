# 🎭 The LazySysAdmin CTF: A Comedy in Three Acts 🎪

## *Or: How I Learned to Stop Worrying and Love the Lazy Admin*

---

## 🎬 Act I: The Discovery (Network Scanning Drama)

**[Scene: Our hero sits down with coffee, ready to hack]**

**Me:** "Alright, time to be an elite hacker! Let me start with a network scan."

```bash
nmap -sn 192.168.101.0/24
```

**Nmap:** "Yo, found 5 hosts! One of them is called... *drumroll* ...LAZYSYSADMIN"

**Me:** "Wait, WHAT? The machine literally called itself LAZY? That's like a bank vault labeled 'PLEASE ROB ME' with the combo written on a sticky note!"

---

## 🎪 Act II: The Port Scan Revelation

**Me:** "Let's see what services are running..."

```bash
nmap -sV -A 192.168.101.15
```

**Nmap Report:**
- Port 22: SSH ✓
- Port 80: Apache ✓
- Port 139: SMB... *wait what?*
- Port 445: SMB AGAIN?! 
- Port 3306: MySQL (unauthorized) 😏
- Port 6667: IRC Server 🤨

**Me:** "Dude literally opened EVERY PORT like it's Black Friday and ports are 90% off!"

**Internal Monologue:** *"This admin is so lazy, I bet their password is literally 'password'..."*

---

## 🎭 Act III: The SMB Share Comedy

**Me:** "Let's check SMB shares. Probably nothing there..."

```bash
smbclient -L //192.168.101.15 -N
```

**SMB Server:** "Here are my shares:
- print$ (boring)
- **share$** (ooh, mysterious!)
- IPC$ (meh)"

**Me:** "Share$? That's as creative as naming your dog 'Dog'. Let's look inside!"

```bash
smbclient //192.168.101.15/share$ -N
```

**SMB Server:** *opens door wide* "WELCOME! Here's literally EVERYTHING!"

**Me:** 😱 "There's a file called `deets.txt`... DEETS?! Who names their sensitive file 'DEETS'?!"

---

## 📜 The "Deets.txt" Incident

**Contents of deets.txt:**
```
CBF Remembering all these passwords.

Remember to remove this file and update your password 
after we push out the server.

Password: 12345
```

**Me:** *spits coffee* 

"12345?! THAT'S THE PASSWORD?! That's the same combination an idiot would have on their luggage!"

**Also Me:** "And they DOCUMENTED IT in a file called DEETS?!"

**Plus Me:** "AND left a reminder to remove the file... BUT NEVER DID?!"

*Chef's kiss* 🤌 

---

## 🔐 The WordPress Config Goldmine

**Me:** "Let me check the WordPress config just for fun..."

```bash
smbclient //192.168.101.15/share$ -N
cd wordpress
get wp-config.php
```

**wp-config.php reveals:**
```php
define('DB_USER', 'Admin');
define('DB_PASSWORD', 'TogieMYSQL12345^^');
```

**Me:** "TOGIE?! The password has someone's NAME in it?! AND MySQL AND a pattern?!"

"This is like a password BINGO card! All we need is their birthday and we'd have a full house!"

---

## 🎯 The Hydra Confirmation

**Me:** "Let me just confirm these creds work on SSH..."

```bash
hydra -l togie -p 12345 ssh://192.168.101.15
```

**Hydra:** *2 seconds later* "Yup! Valid! 🎉"

**Me:** "That was faster than ordering a pizza!"

---

## 🚀 Act IV: The SSH Comedy Hour

```bash
ssh togie@192.168.101.15
Password: 12345
```

**SSH Banner:**
```
##################################################################################################
#                                          Welcome to Web_TR1                                    #
#                             All connections are monitored and recorded                         # 
#                    Disconnect IMMEDIATELY if you are not an authorized user!                   # 
##################################################################################################
```

**Me:** "Ooh, scary banner! 'All connections are monitored!' Sure, Jan. By WHO? The password is 12345!"

---

## 👑 The Sudo Surprise

**Me:** "Let me check sudo permissions..."

```bash
sudo -l
[sudo] password for togie: 12345
```

**System:** "User togie may run the following commands on LazySysAdmin:
    (ALL : ALL) ALL"

**Me:** "ALL?! (ALL : ALL) ALL?! That's not sudo permissions, that's a SUDO INVITATION!"

"This is like giving someone the keys to your house, the code to your safe, AND a map to where you hide your money!"

---

## 🎪 The Root Escalation (30 Second Edition)

```bash
togie@LazySysAdmin:~$ sudo su -
[sudo] password for togie: 12345
root@LazySysAdmin:~# whoami
root
```

**Me:** "That's it?! Just 'sudo su -'?! I've had harder times opening a jar of pickles!"

---

## 🏆 The Victory Lap

```bash
root@LazySysAdmin:~# cat proof.txt
```

**Flag appears:**
```
WX6k7NJtA8gfk*w5J3&T@*Ga6!0o5UP89hMVEQ#PT9851

Well done :)

Hope you learn't a few things along the way.

Regards,
Togie Mcdogie
```

**Me:** "TOGIE MCDOGIE?! The admin's name is literally TOGIE MCDOGIE?! This whole thing was a meme!"

---

## 📊 The Security Audit Report

### What We Learned:

1. **SMB Share with Guest Access** ✓
   - *Translation:* Front door? Who needs one!

2. **Password in Plain Text File** ✓
   - *Translation:* Password sticky note on monitor, digital edition

3. **WordPress Config Accessible** ✓
   - *Translation:* Source code? More like open source code!

4. **Sudo ALL Permissions** ✓
   - *Translation:* "Trust everyone" security model

5. **Password: 12345** ✓✓✓
   - *Translation:* THE MEME IS REAL

---

## 🎬 The Timeline

**0:00** - Start nmap scan
**0:30** - Found target "LazySysAdmin" (should have known)
**1:00** - Port scan (everything is open, literally everything)
**2:00** - SMB enumeration  
**2:30** - Found deets.txt (*facepalm*)
**3:00** - Read password: "12345" (*double facepalm*)
**4:00** - SSH login successful
**4:30** - Discovered sudo (ALL:ALL) ALL (*triple facepalm*)
**5:00** - Got root
**5:30** - Still laughing

**Total Time:** 5 minutes and 30 seconds (including time spent laughing)

---

## 🎓 Lessons Learned

### From the Admin's Perspective:
- ❌ Don't name your machine "LazySysAdmin"
- ❌ Don't leave SMB shares wide open
- ❌ Don't store passwords in files called "deets.txt"
- ❌ Don't use "12345" as a password
- ❌ Don't give users sudo ALL permissions
- ❌ Don't do literally everything Togie Mcdogie did

### From the Hacker's Perspective:
- ✓ Always check SMB shares
- ✓ Always read files with funny names
- ✓ Never underestimate the power of lazy admins
- ✓ 12345 is still a thing in 2025
- ✓ Some CTFs are basically comedy shows

---

## 🎤 The Final Monologue

**Me:** "This CTF taught me that somewhere out there, there's a real sysadmin thinking 'I'll change that password tomorrow' while their server is getting pwned."

"To Togie Mcdogie: Thank you for creating the world's most entertainingly insecure CTF. You've shown us that sometimes the best security lesson is learning what NOT to do."

"Remember kids: If you're going to be lazy, at least don't document it in a file called 'deets.txt'!"

---

## 🏅 Achievement Unlocked!

**Speed Run: Root in Under 6 Minutes** ✓
**Found Password in Plain Text** ✓  
**Sudo All The Things** ✓
**Laughed Out Loud During CTF** ✓✓✓

**Final Score:** 12345/12345 (Coincidence? I think not!)

---

## 📝 TL;DR

1. Scanned network → Found "LazySysAdmin" (red flag #1)
2. Open SMB share → Found "deets.txt" (red flag #2)  
3. Password is "12345" (red flag #3)
4. User has sudo ALL (red flag #4)
5. Got root → Laughed for 10 minutes → Wrote this

**Difficulty:** 1/10 (The hardest part was typing while laughing)

**Realism:** 10/10 (Sadly, this probably exists in the real world)

---

## 🎭 Epilogue

*Somewhere, in a parallel universe, Togie Mcdogie is reading this and thinking:*

**Togie:** "Maybe I SHOULD change that password..."

**Also Togie:** "...Tomorrow."

---

## 🙏 Credits

- **LazySysAdmin CTF Creator:** For teaching us that lazy admins make the best CTFs
- **Password "12345":** For still being relevant after 40 years  
- **SMB Protocol:** For being the gift that keeps on giving
- **Togie Mcdogie:** For being an unforgettable character

---

## 🎪 The End

*This has been a production of "How Not To Secure Your Server: A Masterclass"*

**Remember:** Change your passwords, close your shares, and for the love of all that is holy, don't document your passwords in files called "deets.txt"!

---

**P.S.** If your password is still "12345" after reading this, we need to have a talk.

**P.P.S.** If you have a file called "deets.txt" on your server right now, please delete it.

**P.P.P.S.** No Togie Mcdogies were harmed in the making of this CTF solve.

---

*Written with ❤️ and 😂 by someone who spent 5 minutes hacking and 30 minutes laughing*

**Date:** November 8, 2025  
**Status:** PWNED ✓  
**Mood:** Amused 😄  
**Password Security:** Still Crying 😭
