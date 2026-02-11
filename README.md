# Urai Space 🔐

**Your private, zero-knowledge encrypted journal. Not even we can read your data.**

<p align="center">
  <img src="https://img.shields.io/badge/🔐_Encryption-AES--256--GCM-blue?style=for-the-badge" alt="AES-256-GCM"/>
  <img src="https://img.shields.io/badge/🔑_Key_Derivation-PBKDF2-green?style=for-the-badge" alt="PBKDF2"/>
  <img src="https://img.shields.io/badge/🛡️_Architecture-Zero_Knowledge-red?style=for-the-badge" alt="Zero Knowledge"/>
</p>

---

## 🌟 What is Urai Space?

Urai Space is a **zero-knowledge encrypted journaling application** that puts your privacy first. Write diary entries, take notes, and create drawings - all protected by military-grade encryption that ensures **not even we** can read your data.

### 🎯 Perfect For

- 📔 **Personal journaling** - Daily thoughts, reflections, and memories
- 📝 **Secure note-taking** - Ideas, passwords, sensitive information
- 🎨 **Private sketching** - Drawings and doodles encrypted before storage
- 🔒 **Privacy-conscious users** - Anyone who values true data privacy

---

## 🛡️ Security First

> **"The only truly secure data is data that no one else can read - including us."**

Urai Space is built with **zero-knowledge architecture**. This means:

| What We Store | What We CAN'T See |
|---------------|-------------------|
| Encrypted blobs | Your diary entries |
| Random IVs | Your notes content |
| Salted hashes | Your password |
| | ANY of your personal data |

### 🔐 Encryption Specifications

> **Want a simple explanation?** 
> Check out our **[Security in Plain English](SECURITY.md)** guide to see visual diagrams of how your data is protected.

| Component | Specification | Why It Matters |
|-----------|--------------|----------------|
| **Encryption** | AES-256-GCM | Same encryption used by governments and banks. 2²⁵⁶ possible keys. |
| **Key Derivation** | PBKDF2 with 100,000 iterations | Brute force would take billions of years |
| **Random Salt** | 128-bit cryptographic random | Prevents rainbow table attacks |
| **IV** | Unique per encryption | Same plaintext = different ciphertext |

### 🧮 How Hard Is It To Crack?

The encryption key space is **2²⁵⁶ possible combinations**:

```
115,792,089,237,316,195,423,570,985,008,687,907,853,269,984,665,640,564,039,457,584,007,913,129,639,936
```

To put this in perspective:

| Scenario | Time Required |
|----------|---------------|
| 1 billion keys/second | **3.67 × 10⁶⁰ years** |
| All computers on Earth combined | **Still billions of years** |
| Age of the universe (~13.8 billion years) | **Not even close** |

**The sun will burn out before anyone cracks your diary.**

### 🏗️ Security Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     YOUR DEVICE ONLY                         │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  🔑 Master Password                                    │  │
│  │      ↓                                                 │  │
│  │  PBKDF2 (100,000 iterations) + Unique Salt            │  │
│  │      ↓                                                 │  │
│  │  ┌────────────────┐    ┌────────────────────────┐     │  │
│  │  │ Auth Key       │    │ Encryption Key         │     │  │
│  │  │ (verification) │    │ (encrypts your data)   │     │  │
│  │  └────────────────┘    └────────────────────────┘     │  │
│  │                              ↓                         │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │ 📔 Diary → encrypt() → ciphertext               │  │  │
│  │  │ 📝 Notes → encrypt() → ciphertext               │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────────┘  │
│                              ↓                               │
│              Only encrypted data leaves your device          │
└──────────────────────────────────────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                        ☁️ CLOUD STORAGE                      │
│                                                              │
│   Stores: salt, auth_hash, iv, ciphertext                   │
│   Cannot read: ANYTHING about your actual content            │
│                                                              │
│   Even if hacked, attackers get: meaningless encrypted bytes │
└──────────────────────────────────────────────────────────────┘
```

### 🔒 What Makes Us Different

| Feature | Other Apps | Urai Space |
|---------|------------|------------|
| Password storage | Hashed on server | **Never leaves your device** |
| Encryption | Server-side | **Client-side (your browser)** |
| Who can read data | Company employees | **Only you** |
| Government requests | Can comply | **Mathematically impossible** |
| Data breach impact | Your data exposed | **Only encrypted gibberish** |

### 🚨 Recovery & Account Security

- **12-word recovery phrase** - Write it down, store it safely.
- **"Lockbox" Mechanism** - Your 12 words encrypt a copy of your Master Key. If you forget your password, the 12 words unlock this box.
- **Password Reset** - When you reset your password using the recovery phrase, the app:
    1. Decrypts your data **locally on your device** (in memory).
    2. Generates a NEW key from your NEW password.
    3. Re-encrypts all your data with the new key.
    4. Updates the "Lockbox" so your **same 12 words** still work.
    
    > **Data never leaves RAM:** Your data is decrypted for a split second in your device's memory (RAM), re-encrypted immediately with the new key, and only the newly encrypted "gibberish" is sent back to the database. We cannot see it.
- **No backdoors** - We (the developers) cannot reset your password or recover your data without your phrase.

---

## ✨ Features

### 📔 Encrypted Diary
- Date-based entries with calendar navigation
- All content encrypted before storage
- Cloud sync across devices

### 📝 Secure Notes  
- Google Keep-style colorful tiles
- 8 color options
- Create, edit, delete - all encrypted
 
### 🎨 Creative Canvas
- **Encrypted Drawings** - Sketches are encrypted before upload
- **Professional Tools** - Pen, eraser, 3 brush sizes
- **Neon Palette** - 6 curated colors
- **Gallery Mode** - Grid view with bulk management

### 👥 Multi-Account
- Multiple accounts per device
- Complete data isolation
- Switch accounts easily
- Each account has unique encryption keys

### ⚙️ User Control
- **Change Password**: Rotate your master key without losing data (thanks to Key Wrapping)
- **Export Data**: Download a decrypted backup of all your data (JSON format, password protected)
- **Delete Account**: Permanently wipe all your data from the server

### 🎨 Modern UI
- Dark/Light mode
- Glassmorphism design
- Responsive on all devices

---

## 🚀 Get Started

Visit **[Your App URL Here]** to start using Urai Space today!

### First Time Setup

1. **Create Account** - Choose a strong master password
2. **Save Recovery Phrase** - Write down your 12-word phrase (you'll need this if you forget your password)
3. **Start Writing** - Your data is encrypted automatically

---

## 📱 How to Use

### Creating Your First Entry

1. Click the **Diary** tab
2. Select a date from the calendar
3. Write your entry
4. It's automatically saved and encrypted!

### Taking Secure Notes

1. Click the **Notes** tab
2. Click **"Create Note"**
3. Choose a color and start typing
4. All notes are encrypted before storage

### Drawing & Sketching

1. Click the **Draw** tab
2. Use the pen tool to create sketches
3. Choose from 6 neon colors
4. Switch to gallery mode to view all your drawings

### Managing Multiple Accounts

1. Click the **lock icon** to sign out
2. Choose **"Switch account"** or **"Create new account"**
3. Each account is completely isolated with its own encryption

---

## ❓ FAQ

### What happens if I forget my password?

Use your **12-word recovery phrase** to reset your password. Make sure you've saved it somewhere safe!

### Can you recover my data if I lose both my password and recovery phrase?

**No.** Because of the zero-knowledge architecture, we have absolutely no way to decrypt your data. This is by design - it's what keeps your information truly private.

### What if there's a data breach?

Attackers would only get encrypted gibberish. Without your password, the data is mathematically impossible to decrypt (would take billions of years).

### Can I use Urai Space on multiple devices?

Yes! Your encrypted data syncs across all devices. Just sign in with your email and password.

### Is my data really private from you?

**Absolutely.** All encryption happens in your browser before data is sent to our servers. We only store encrypted blobs that we cannot read.

### What technology powers the encryption?

- **AES-256-GCM** - Industry standard encryption
- **PBKDF2** - Key derivation with 100,000 iterations
- **Web Crypto API** - Browser-native cryptography

---

## 🔗 Links

- **Security Details** - See [SECURITY.md](SECURITY.md) for technical specifications
- **Changelog** - See [CHANGELOG.md](CHANGELOG.md) for version history
- **Feedback** - [Report a Bug](https://github.com/ramprasanth0/urai-space-guide/issues/new?template=bug_report.md) or [Request a Feature](https://github.com/ramprasanth0/urai-space-guide/issues/new?template=feature_request.md)

---

## ⚠️ Important Security Notes

1. **Your password is your only key** - We cannot recover it
2. **Save your 12 words** - They're your backup
3. **Clear browser data = lose local access** - But you can restore with password
4. **We recommend a strong, unique password** - It protects everything

---

<p align="center">
  Made with 🔐 for privacy-conscious individuals
  <br>
  <sub>*"To find the silence, one must strike the heart of the space five times. 🌌"*</sub>
</p>
