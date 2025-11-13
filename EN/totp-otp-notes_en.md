# **TOTP / HOTP One-Time Password Notes (English Version)**

### A simple way to explain TOTP

Imagine two agents starting their stopwatches at the exact same moment:

* “Ready… set… go!” → This moment represents *sharing the secret key*.
* Both agents then operate separately, but their stopwatches keep ticking under the same rules.
* When they meet again:
  “Prove you're real — tell me your stopwatch time.”
* If the numbers match: **real**
* If not: **fake**

This is the core concept behind TOTP.

---

## 1. One-Time Password Methods

### **TOTP (Time-based One-Time Password)**

* Changes every 30 seconds
* Most widely used today
* Used by Google Authenticator, Microsoft Authenticator, etc.

### **HOTP (HMAC-based One-Time Password)**

* Increments by counter instead of time
* Mostly used in older hardware token systems

### **Both are defined in public RFC specifications**

* Completely open and free to implement
* Libraries are available in most languages
* Very low implementation cost

---

## 2. How TOTP is Implemented

### **Uses open algorithms**

* The server implements TOTP/HOTP logic
* Typically done via existing libraries
* Google Authenticator is just a *client app*, not the core system

### **Secret key generation (most important)**

* Server generates a random secret key per user
* Both server and device store this secret
* Both independently compute the TOTP value from the same key

---

## 3. What the Authenticator App Does

### **Its behavior is very simple:**

1. Stores the secret key securely
2. Reads the current UNIX time
3. Computes `step = floor(UNIX_time / 30)`
4. Runs `HMAC(secret, step)`
5. Displays the 6-digit code

### **Users never see the secret key**

* Stored in the OS secure storage
* Practically impossible for external apps to read

---

## 4. Server-Side Behavior

### **The server computes TOTP only when needed**

1. User enters the 6-digit code
2. Server checks its own current time
3. Computes the same `step`
4. Runs `HMAC(secret, step)`
5. If the values match → authenticated

### **Very lightweight**

* Server does not compute continuously
* Only runs when a user logs in

---

## 5. Time Synchronization

### **Global standard: UTC**

* Devices and servers eventually sync to UTC
* UNIX time is simply “seconds from 1970-01-01 UTC”

### **Smartphones**

* Keep time internally
* Regularly corrected via NTP or carrier signals
* Small drift is acceptable

### **Servers**

* Continuously synced with NTP
* Drift is nearly zero in production systems

### **Accepted time drift**

* Usually ±30 seconds is allowed
* Matches TOTP’s 30-second window

---

## 6. Security Fundamentals

### **Strength comes from protecting the secret key**

* If the secret is leaked → TOTP is broken
* If the secret is protected → extremely secure
* Device → secure enclave
* Server → encrypted database storage

### **Users never touch the secret**

* Only see the generated 6-digit codes

### **Possible attack surface**

* Malicious authenticator app (rare)
* Not through the communication channel (secret is never transmitted)

---

## 7. Benefits of TOTP

* Free to implement
* Uses open and trusted standards
* Trusted by financial institutions
* Works offline
* Scales easily with large numbers of users
* Very low computational cost
* Excellent security-focused impression for development projects

---

## **Summary**

* TOTP/HOTP are RFC-defined open standards
* Secret keys are shared once and stored securely
* Both sides compute the same result from
  `HMAC(secret, floor(UNIX_time / 30))`
* Human-friendly 6-digit codes
* Robust even with time drift
* Zero licensing cost and highly secure