# CYBHI Incident Reporting/Counselling  
Security Architecture

## Executive Summary

This architecture ensures that no student‑identifiable information (PII) is ever exposed to Evolo AI systems, backend services, or developers. All PII is redacted on the client before any data is sent to the Large Language Model (LLM). The LLM receives only anonymized placeholders (e.g., {{STUDENT_01}}) and never processes or stores real names, IDs, or other sensitive attributes.

Each placeholder maps to an asymmetrically encrypted Name and ID, encrypted using a Public Key that is itself encrypted before being stored in the backend. The corresponding Private Key remains exclusively on the School Administrator’s secure vault or iOS keychain. Evolo AI never receives this private key and therefore cannot decrypt any PII.

Because of this architecture:

- Evolo AI cannot decrypt or view any student information.
- PII can only be decrypted locally by authorized users (Incident Reporters, Counselors, Admins).
- Even if someone accesses the LLM output, backend database, or logs, placeholders cannot be linked to real students.

This design provides end‑to‑end encryption, zero‑knowledge guarantees, and role‑based decryption, while still enabling complete and compliant CYBHI billing workflows.

## 1\. Architectural Goal

The system is designed to ensure:

- Only Incident Reporters, Counselors, and Admins can decrypt sensitive data (PII, FERPA‑protected information).
- Backend services, developers, and general staff never access plaintext PII.
- Even in the event of a database breach, all data remains encrypted and unusable.
- Full compliance with FERPA, HIPAA, GDPR, and CYBHI billing requirements.
- Seamless integration with CYBHI billing systems while maintaining strict privacy boundaries.

## 2\. Key Principles

| **Principle** | **Implementation** |
| --- | --- |
| Zero‑knowledge | Backend never sees plaintext keys or decrypted data. |
| --- | --- |
| End‑to‑end encryption | All encryption occurs on the client before data is transmitted. |
| --- | --- |
| Role‑based decryption | Only Incident Reporters, Counselors, and Admins hold private keys. |
| --- | --- |
| Dual‑key security | Access requires both account login and private key. |
| --- | --- |
| Secure storage | Private keys stored in OS keychain / secure enclave. |
| --- | --- |
| Data redaction | PII stripped before LLM processing; only placeholders sent. |
| --- | --- |
| Backend isolation | Backend stores only encrypted blobs and encrypted keys. |
| --- | --- |

## 3\. Encryption Model

### 3.1 Key Types

- **Public Key (AES‑256)**
    - Generated on the frontend during user registration.
    - Encrypts/decrypts user data.
    - Never stored in plaintext; only stored encrypted.

- **Private Key (User‑chosen)**
    - Human‑memorable passphrase.
    - Strengthened using PBKDF2 or Argon2id.
    - Used to decrypt the Public Key.
    - Never transmitted to backend.

- **Master Data Key (DMK)**
    - AES‑256 symmetric key used to encrypt all user data.
    - Decrypted only by authorized users.

## 3.2 Encryption Flow

**Code**

**\[User Private Key\]**

**↓ decrypts**

**\[Encrypted Public Key\]**

**↓ decrypts**

**\[Master Data Key (DMK)\]**

**↓ decrypts**

**\[Encrypted User Data\]**

**All cryptographic operations occur on the client.**

# **4\. User Access Model**

### **Incident Reporters & Counselors**

- **Submit encrypted incident or counseling data.**
- **Can decrypt only their own submissions.**
- **Cannot access other users’ data unless explicitly granted by Admin.**

### **Admins**

- **Hold private keys capable of decrypting the DMK.**
- **Can decrypt all data for compliance, reporting, and CYBHI billing.**
- **Can issue temporary access tokens for limited staff access.**

### **Backend / Developers / General Staff**

- **Cannot decrypt any PII.**
- **Only see encrypted blobs and redacted placeholders.**
- **Zero ability to reconstruct student identities.**

# **5\. Security Scenarios**

| **Scenario** | **Attacker Has Account?** | **Attacker Knows Private Key?** | **Can Access Encrypted Key?** | **Can Decrypt Data?** |
| --- | --- | --- | --- | --- |
| **Outsider** | **No** | **No** | **No** | **No** |
| --- | --- | --- | --- | --- |
| **Only private key** | **No** | **Yes** | **No** | **No** |
| --- | --- | --- | --- | --- |
| **Only account login** | **Yes** | **No** | **Yes** | **No** |
| --- | --- | --- | --- | --- |
| **Database leaked** | **No** | **No** | **Yes** | **No** |
| --- | --- | --- | --- | --- |
| **Developer access** | **Yes** | **No** | **Yes** | **No** |
| --- | --- | --- | --- | --- |
| **User forgets private key** | **Yes** | **Lost** | **Yes** | **No (data lost)** |
| --- | --- | --- | --- | --- |

**This table demonstrates the zero‑knowledge nature of the system: Two factors (account + private key) are always required.**

# **6\. Implementation Notes**

- **All encryption/decryption occurs on the frontend.**
- **Backend stores only:**
    - **encryptedPublicKey**
    - **encryptedUserData**
    - **userId**
- **No plaintext PII ever leaves the device.**
- **Audit logs track all decryptions for compliance.**
- **Admins may maintain backup private keys for recovery.**

# **7\. Advantages**

- **End‑to‑end encryption ensures PII is never exposed.**
- **Role‑based access restricts decryption to authorized personnel.**
- **Zero‑knowledge backend protects against developer access and database breaches.**
- **FERPA/HIPAA/GDPR compliant by design.**
- **CYBHI billing‑ready while maintaining strict privacy boundaries.**

# **8\. CYBHI Example Scenario: Substance Use Screening & Counseling (Code 99408)**

### **Scenario Overview**

**A high school counselor documents a 45‑minute substance use screening and brief intervention session. This service is billable under CYBHI code 99408 and must remain encrypted end‑to‑end.**

## **Step‑by‑Step Flow**

### **1\. Counselor Initiates Report**

- **Counselor opens the CYBHI app.**
- **Student identifiers (name, ID, DOB) are automatically redacted before any LLM processing.**
- **Counselor enters session notes (e.g., discussion of alcohol experimentation and coping strategies).**

### **2\. Frontend Encryption**

- **App generates a random AES‑256 Public Key for the counselor at registration.**
- **Counselor’s Private Key (derived via PBKDF2/Argon2id) decrypts the Public Key.**
- **Session notes + redacted identifiers are encrypted with the DMK before leaving the device.**

### **3\. Backend Storage**

**Backend receives only:**

- **Encrypted session data**
- **Encrypted Public Key**
- **User ID**

**Backend never sees plaintext PII or decrypted notes.**

### **4\. Admin Oversight & Billing Integration**

- **Admin logs in with their Private Key.**
- **Admin decrypts the DMK → decrypts counselor’s session data.**
- **Admin re‑inserts actual student identifiers for billing completeness.**
- **Billing system receives:**
    - **CYBHI code 99408**
    - **Counselor notes**
    - **Student identifiers (locally decrypted)**

### **5\. Security Scenarios Applied**

- **Database leak: Only encrypted blobs exposed; no decryption possible.**
- **Developer access: Developers see ciphertext only.**
- **Lost counselor key: Admin can still decrypt via DMK.**
- **Lost admin key: All data becomes unrecoverable (zero‑knowledge).**

### **6\. Audit & Compliance**

- **All decryptions logged.**
- **FERPA/HIPAA compliance maintained.**
- **CYBHI billing receives complete, accurate, compliant data.**

# **Conclusion**

**This architecture provides a robust, zero‑knowledge, end‑to‑end encrypted system that protects student PII while enabling complete and compliant CYBHI billing workflows. It ensures that only authorized personnel — Incident Reporters, Counselors, and Admins — can decrypt sensitive data, while Evolo AI and backend systems remain fully isolated from PII.**

**It is a secure, scalable, and standards‑aligned solution that meets both operational and regulatory requirements.**

**Executive Summary:**  
All personal information (PII) is redacted before any data is sent to the Large Language Model (LLM). The LLM only receives redacted content that contains non-identifiable placeholders instead of real names or IDs. This ensures the model never processes or stores student-identifiable information.

The generated report from LLM will include placeholders (e.g., {{STUDENT_01}}). These placeholders map to only asymmetric encrypted Name and ID (Any other PII data will be redacted). The private key will remain on School Adminnistrator Secure vault or ios key vault. The public key will be encrypted and provided to evolo ai.

Because of this architecture:

- Evolo AI _cannot decrypt or view_ any student information.  
    
- PII can only be retrieved and decrypted locally by the reporter or admin when needed for reporting completeness.  
    
- Even if someone accesses the LLM output, database, or logs, the placeholders alone cannot identify any student.

## **Goal**

**Objective: Ensure that only the reporter and school admin admin can decrypt sensitive data (PII, FERPA).**

- **Backend, developers, and staff cannot access plaintext.  
    **
- **Even if the database is compromised, data remains encrypted.  
    **
- **Supports audit and compliance requirements.  
    **

- **Types of Keys:**
    - Public Key (AES-256)
        - Generated randomly on the frontend when a user registers.
        - Encrypts/decrypts actual user data.
        - Never stored in plaintext—only stored encrypted in the database.
    - Private Key (small, human-memorable)
        - Chosen by the user (like a password).
        - Used to encrypt/decrypt the Public Key.
        - Never stored on the backend.
- **Backend Storage:**  
    Only stores:
    - user_data
    - encryptedPublicKey
- Developers or attackers cannot access plaintext keys.

### **2\. How Keys Are Generated and Stored**

1.  User registers → frontend generates a random AES Public Key.
2.  User enters a Private Key.
3.  Frontend strengthens the private key using PBKDF2 (derives a secure key from the password + salt).
4.  Frontend encrypts the Public Key with this derived key.
5.  Frontend sends to backend:
    - encryptedPublicKey
    - userId
6.  Backend stores the encrypted Public Key along with user data.

At no point does the backend see the user’s Private Key or the plaintext Public Key.

### **3\. How Security Works**

There are two layers of security:

1.  **Account Access Layer:**  
    Each encrypted public key is tied to a user account. Even if someone tries to steal it, they must log in as that user.
2.  **Cryptographic Layer:**  
    Even with the encrypted key, an attacker cannot decrypt it without the correct Private Key.

Result: To access data, an attacker needs both the user account and the user’s Private Key.

### **4\. Security Scenarios**

| Scenario | Attacker Has Account? | Attacker Knows Private Key? | Can They Access Encrypted Key? | Can They Decrypt Data? |
| --- | --- | --- | --- | --- |
| Outsider | No  | No  | No  | No  |
| --- | --- | --- | --- | --- |
| Only private key | No  | Yes | No  | No  |
| --- | --- | --- | --- | --- |
| Only account login | Yes | No  | Yes | No  |
| --- | --- | --- | --- | --- |
| Database leaked | No  | No  | Yes | No  |
| --- | --- | --- | --- | --- |
| Developer access | Yes | No  | Yes | No  |
| --- | --- | --- | --- | --- |
| User forgets Private Key | Yes | Lost | Yes | No (data lost) |
| --- | --- | --- | --- | --- |

### 5\. Key Principles

1.  Backend Never Holds Real Keys:  
    Only encrypted keys are stored. Developers cannot decrypt them.
2.  Frontend Does All Cryptography:  
    Encryption and decryption happen only on the client side.
3.  Zero-Knowledge Security:  
    If a user loses their Private Key, their data is gone forever. Backend cannot recover it.
4.  Two Keys Required for Access:
    - Account login
    - Private Key

This is similar to WhatsApp’s end-to-end encryption or iCloud zero-knowledge vaults.

### 6\. User Flow

- Admin Register:
    1.  Enters their Private Key → Public Key is generated and stored encrypted on backend.
    2.  Admin can have backup private keys incase the forgot one.
- Classified Staff Register:
    1.  Requests access → Admin approves.
    2.  Admin provides a one-time Private Key.
    3.  Staff logs in with one-time key and sets their own Private Key.
    4.  Classified Staff can request admin to reset private key if the forgot

Every user needs both keys to access data. Developers cannot access any information because the backend only has one part of the key.
