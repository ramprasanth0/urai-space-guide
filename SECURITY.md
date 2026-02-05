# 🛡️ Security Documentation

This document provides detailed technical information about Urai Space's security architecture and cryptographic implementation.

---

## 🔐 Zero-Knowledge Architecture

Urai Space implements a **true zero-knowledge architecture**, meaning:

- **All encryption happens client-side** (in your browser)
- **Your password never leaves your device**
- **We cannot decrypt your data** - mathematically impossible without your password
- **No backdoors or master keys** - not even for developers or law enforcement

---

## 🔑 Cryptographic Specifications

### Encryption Algorithm

**AES-256-GCM (Advanced Encryption Standard, 256-bit, Galois/Counter Mode)**

- **Key Size**: 256 bits (32 bytes)
- **Block Size**: 128 bits (16 bytes)
- **Mode**: GCM (provides both confidentiality and authenticity)
- **IV Size**: 96 bits (12 bytes), randomly generated per encryption
- **Tag Size**: 128 bits (provides authentication)

**Why AES-256-GCM?**
- Used by governments and financial institutions worldwide
- NIST approved and FIPS 140-2 compliant
- Provides authenticated encryption (prevents tampering)
- Resistant to all known practical attacks

### Key Derivation

**PBKDF2-SHA256 (Password-Based Key Derivation Function 2)**

- **Algorithm**: PBKDF2 with SHA-256
- **Iterations**: 100,000 (exceeds OWASP recommendations)
- **Salt**: 128-bit cryptographically random salt (unique per user)
- **Output**: Two 256-bit keys (authentication key + encryption key)

**Why PBKDF2?**
- Deliberately slow to prevent brute-force attacks
- 100,000 iterations makes each password attempt computationally expensive
- Unique salt prevents rainbow table attacks

### Random Number Generation

**Cryptographically Secure Random Number Generator (CSPRNG)**

- Uses browser's `crypto.getRandomValues()`
- Provides cryptographically strong random values
- Used for: salts, initialization vectors (IVs), recovery phrases

---

## 🏗️ Security Architecture Flow

### Account Creation

```
1. User enters password
   ↓
2. Generate random 128-bit salt
   ↓
3. PBKDF2(password + salt) → 64 bytes
   ├─ First 32 bytes  → Authentication Key
   └─ Second 32 bytes → Encryption Key
   ↓
4. Hash(Authentication Key) → Stored on server
   (Original authentication key discarded)
   ↓
5. Encryption Key → Stays in browser memory ONLY
   ↓
6. Generate 12-word recovery phrase
   ↓
7. Encrypt Encryption Key with recovery phrase
   ↓
8. Store encrypted lockbox on server
```

**What's stored on server:**
- Salt (public, safe to share)
- Auth hash (cannot be reversed to get password)
- Encrypted lockbox (cannot be decrypted without recovery phrase)

**What's NOT stored anywhere:**
- Your actual password
- Your encryption key (exists only in browser memory)

### Data Encryption

```
1. User creates diary entry/note/drawing
   ↓
2. Generate random 96-bit IV
   ↓
3. AES-256-GCM encrypt with:
   - Plaintext: Your content
   - Key: Encryption key (from memory)
   - IV: Random IV
   ↓
4. Output: IV + Ciphertext + Authentication Tag
   ↓
5. Send to server (all components transmitted together)
   ↓
6. Plaintext is immediately cleared from memory
```

### Data Decryption

```
1. Fetch encrypted data from server
   ↓
2. Extract: IV + Ciphertext + Tag
   ↓
3. AES-256-GCM decrypt with:
   - Ciphertext: Encrypted content
   - Key: Encryption key (from memory)
   - IV: Stored IV
   ↓
4. Verify authentication tag
   ↓
5. Display plaintext to user
   ↓
6. Plaintext exists only in memory, never persisted
```

### Password Reset (Recovery Phrase)

```
1. User provides 12-word recovery phrase
   ↓
2. Fetch encrypted lockbox from server
   ↓
3. Derive key from recovery phrase (client-side)
   ↓
4. Decrypt lockbox → Original Encryption Key
   ↓
5. Fetch ALL encrypted data from server
   ↓
6. Decrypt each item (in memory, one at a time)
   ↓
7. User enters NEW password
   ↓
8. Generate NEW salt and NEW Encryption Key
   ↓
9. Re-encrypt each item with NEW key
   ↓
10. Create NEW encrypted lockbox
    ↓
11. Upload re-encrypted data + new auth hash + new lockbox
    ↓
12. Old keys cleared from memory
```

**Important:** During password reset, your data exists briefly in RAM during re-encryption. It is **never** transmitted in plaintext and is immediately cleared after re-encryption.

---

## 🔢 Mathematical Security

### Key Space

AES-256 has **2²⁵⁶ possible keys**:

```
115,792,089,237,316,195,423,570,985,008,687,907,853,269,984,665,640,564,039,457,584,007,913,129,639,936
```

**Brute Force Analysis:**

| Attack Rate | Time to Break |
|-------------|---------------|
| 1 million keys/sec | 3.67 × 10⁶³ years |
| 1 billion keys/sec | 3.67 × 10⁶⁰ years |
| 1 trillion keys/sec | 3.67 × 10⁵⁷ years |

**For perspective:** The universe is only ~1.38 × 10¹⁰ years old.

### PBKDF2 Protection

With 100,000 iterations:
- Each password attempt takes ~100ms
- Brute forcing a 12-character password with 95 possible characters per position:
  - Attempts needed: 95¹² = 540,360,087,662,636,962,890,625
  - Time at 10 attempts/sec: **1.71 × 10¹⁵ years**

---

## 🚨 Threat Model

### What We Protect Against

✅ **Server Compromise**
- Attacker gains access to database
- **Result:** Only encrypted gibberish obtained (useless without password)

✅ **Network Interception**
- Man-in-the-middle attack on data transmission
- **Result:** Only encrypted data transmitted (TLS provides additional layer)

✅ **Insider Threat**
- Malicious employee/developer
- **Result:** Cannot access data without user's password

✅ **Legal Requests**
- Government/court order to provide data
- **Result:** Can only provide encrypted blobs (mathematically impossible to decrypt)

✅ **Password Reuse Attack**
- User's password leaked from another service
- **Result:** Unique salt prevents rainbow tables; PBKDF2 slows brute force

### What We Cannot Protect Against

❌ **Keylogger/Malware on User Device**
- If user's computer is compromised, attacker can capture password as typed
- **Mitigation:** Use trusted devices, antivirus, keep OS updated

❌ **User Shares Password**
- If user voluntarily gives password to someone
- **Mitigation:** User education about password security

❌ **Weak Password**
- User chooses "password123"
- **Mitigation:** Password strength indicator, recommendations

❌ **Browser Extension Malware**
- Malicious extension reading page content
- **Mitigation:** Use trusted extensions only

❌ **Physical Device Access**
- Someone gains physical access to unlocked device
- **Mitigation:** Lock screen feature, device lock

---

## 🔍 Security Best Practices

### For Users

1. **Use a Strong Password**
   - At least 12 characters
   - Mix of uppercase, lowercase, numbers, symbols
   - Don't reuse passwords from other services

2. **Save Your Recovery Phrase**
   - Write it down on paper
   - Store in multiple secure locations
   - Never share it with anyone

3. **Secure Your Device**
   - Use antivirus software
   - Keep OS and browser updated
   - Don't install untrusted software

4. **Lock Your Session**
   - Use the lock feature when stepping away
   - Enable device screen lock

### For Developers/Auditors

The private code repository contains the full implementation. Key areas to review:

- `/src/lib/crypto.js` - Encryption implementation
- `/src/lib/supabase.js` - Database interaction
- `/src/components/LockScreen.jsx` - Authentication flow

---

## 🆘 Incident Response

### If You Suspect Your Password Is Compromised

1. Sign in to Urai Space immediately
2. Use password reset with your recovery phrase
3. Choose a new, strong password
4. Your data will be re-encrypted with the new key

### If You Lose Your Recovery Phrase

1. **While still signed in:** Go to settings and view your recovery phrase
2. Write it down immediately
3. Store in multiple secure locations

### If You Lose Both Password AND Recovery Phrase

**Your data cannot be recovered.** This is by design - it's what makes the encryption secure. There is no backdoor or master key.

---

## 📊 Security Audit Status

- **Last Security Review:** [Date]
- **Penetration Testing:** [Status]
- **Third-Party Audit:** [Status/Link]

---

## 🔗 References

- [NIST SP 800-38D](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-38d.pdf) - GCM Specification
- [RFC 8018](https://tools.ietf.org/html/rfc8018) - PBKDF2 Specification
- [OWASP Password Storage](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [Web Crypto API](https://www.w3.org/TR/WebCryptoAPI/)

---

## 📧 Reporting Security Vulnerabilities

If you discover a security vulnerability, please email: **[your-security-email@example.com]**

Please do NOT open a public issue for security vulnerabilities.

---

<p align="center">
  🔐 Your privacy is our priority
</p>
