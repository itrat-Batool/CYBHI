# CYBHI Incident Reporting/Counselling  
Security Architecture

## Executive Summary

Our security architecture is built on a strict zero‑knowledge, end‑to‑end encryption model in which Evolo AI systems, backend services, and developers never have access to Student PII at any point in the data lifecycle. All sensitive data is encrypted on the client device, decrypted only by authorized school personnel, and never exposed to the backend. This ensures that even in the event of a breach, misconfiguration, or insider access, Student PII remains cryptographically protected and inaccessible to Evolo AI or any unauthorized party.

This architecture ensures that no Student PII is ever exposed to Evolo AI systems, backend services, or developers. All PII is redacted before any data is sent to the Large Language Model (LLM). The LLM receives only anonymized placeholders (e.g., {{STUDENT_01}}) and never processes or stores real names, IDs, or other sensitive attributes.

As part of system setup, the School Administrator generates a secure, digitally‑signed, one‑time‑upload link for submitting the student roster. This link can be used only once, expires after 15 minutes or immediately upon successful upload, and ensures that roster submission cannot be intercepted, reused, or accessed by unauthorized parties.

When the admin opens the link, the roster (containing each student’s name, school, Student ID, Medical Record ID) is encrypted locally on the admin’s device using the same asymmetric encryption model applied throughout the system. Only encrypted roster entries are transmitted and stored in the backend, and Evolo AI never receives the private key required to decrypt them.

Once the encrypted roster is stored, the system can automatically filter and show the available students based on the incident report or counseling session context. The Incident Reporter, Counselor, or Administrator must explicitly approve each student ID selected from this narrowed list. No mapping is ever auto‑accepted. Students not found in roster (new students or transfers) will be sent to Admin, Counselor or Incident Reporter for manual input.

After approval, each placeholder is linked to an asymmetrically encrypted Name, Class, and Paradigm ID. The associated Private Key remains exclusively on the School Administrator’s secure vault or iOS keychain, ensuring that only authorized users can decrypt student identities.

This design provides end‑to‑end encryption, zero‑knowledge guarantees, human‑in‑the‑loop validation, and role‑based decryption, while still enabling complete and compliant CYBHI billing workflows. CYBHI billing mappings are always human‑approved.

## 1\. Architectural Goal

The system is designed to ensure:

- Only Incident Reporters, Counselors, and Admins can decrypt Student PII.
- Backend services, developers, and general staff never access plaintext Student PII.
- Even in the event of a database breach, all data remains encrypted and unusable.
- Full compliance with FERPA and HIPAA.
- Seamless integration with CYBHI billing systems while maintaining strict privacy boundaries.
- Human‑in‑the‑loop approval for all student ID selections.

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
| Human‑in‑the‑loop | All student ID selections require explicit human approval. |
| --- | --- |
| One‑time secure upload | Roster uploaded via digitally‑signed, single‑use, 15‑minute link. |
| --- | --- |

## 3\. Encryption Model

### 3.1 Key TypesPublic Key (AES‑256)

- Generated on the frontend during user registration.
- Encrypts all user‑submitted PII and admin‑uploaded roster data.
- Never stored in plaintext; only encrypted form stored in backend.

### Private Key (User‑chosen)

- Human‑memorable passphrase strengthened using PBKDF2.
- Used to decrypt the Public Key.
- Never transmitted to backend.

## 3.2 Roster Encryption & Upload Flow

- Admin generates a digitally‑signed, one‑time‑upload link.
- Link expires after 15 minutes or after one successful upload.
- Admin opens the link and selects the roster file.
- Roster is parsed and encrypted locally using the admin’s decrypted Public Key.
- Only encrypted roster entries are transmitted to backend.
- Upload link becomes invalid and cannot be reused.

## 3.3 Student ID Selection Flow

- Reporter or Counselor submits a report.
- PII is redacted and replaced with placeholders.
- The system automatically filters and displays available students based on context.
- User selects the correct student ID from the filtered list.
- The Incident Reporter, Counselor, or Admin must explicitly approve the selected student ID.
- CYBHI billing mappings are stored only after human approval.

## 3.4 Encryption Flow

## 4\. User Access ModelIncident Reporters

- Incident reporter enters full narrative locally.
- PII is redacted and replaced with placeholders.
- System filters and displays relevant students.
- Reporter selects and approves the correct student ID.

### Counselor Session & Documentation

- Counselor receives narrative with placeholders along with Encrypted User information.
- Counselor decrypts the information locally to see the full narrative.
- Counselor enters full session details locally.
- PII is redacted and replaced with placeholders.
- Counselor selects and approves the correct student ID.

## Admins

- Generate secure, digitally‑signed, one‑time‑upload links.
- Upload roster within 15 minutes or before link expires.
- Hold private keys capable of decrypting the public key.
- Review and approve mappings across all reports.
- Request updates if errors are identified.
- Reset passwords.
- Revoke access to accounts in case of lost devices.
- Maintain audit logs and compliance oversight.

## Backend / Developers / General Staff

- Cannot decrypt any Student PII.
- Only see encrypted blobs and placeholders.
- Zero ability to reconstruct student identities.
- Cannot approve mappings.

## 5\. Threat Scenarios

| **Scenario** | **Has Account?** | **Knows Private Key?** | **Can Access Encrypted Key?** | **Can Decrypt Data?** |
| --- | --- | --- | --- | --- |
| Outsider | No  | No  | No  | No  |
| --- | --- | --- | --- | --- |
| Outsider with only private key | No  | Yes | No  | No  |
| --- | --- | --- | --- | --- |
| Only account login | Yes | No  | Yes | No  |
| --- | --- | --- | --- | --- |
| Database leaked | No  | No  | Yes | No  |
| --- | --- | --- | --- | --- |
| Developer access | Yes | No  | Yes | No  |
| --- | --- | --- | --- | --- |
| User forgets private key | Yes | Lost | Yes | No  |
| --- | --- | --- | --- | --- |

This table demonstrates the zero‑knowledge nature of the system: Two factors (account + private key) are always required.

## 6\. Implementation Notes

- All encryption/decryption occurs on the frontend.
- Backend stores only:
    - encryptedPublicKey
    - encryptedUserData
    - encryptedRosterData
- No plaintext Student PII ever leaves the device.
- Audit logs track all decryptions and mapping approvals.
- Admins may maintain backup private keys for recovery.

## 7\. Advantages

- Secure, digitally‑signed, one‑time‑use roster upload link prevents unauthorized access.
- Time‑bound (15‑minute) expiration reduces the attack surface.
- End‑to‑end encryption ensures Student PII is never exposed.
- Role‑based access restricts decryption to authorized personnel.
- Human‑in‑the‑loop approval ensures accuracy and prevents misidentification.
- Zero‑knowledge backend protects against developer access and database breaches.
- FERPA/HIPAA compliant by design.
- CYBHI billing mappings are always human‑approved.

## 8\. CYBHI Security Architecture Applied to Scenario H2014

### Scenario Description

## **Service Category:** Psychoeducation **Code:** H2014 – Skills Training & Development (Individual, 15 min)**Incident Report (Officer Ramirez):**“It’s 3rd period in the east hallway. Jordan Martinez, 9th grade, was yelling at another student, slammed his backpack against the wall, refused to go back to class. He was pacing, fists clenched, looked really upset. No one got hurt, but it was loud and disruptive. I walked him down to the office so he could cool off. He said the other kid keeps making fun of him and he’s tired of it.”**SOAP Note (Counselor):**S: “He keeps making fun of me in front of everyone. I can’t take it anymore.”O: Pacing, yelling, fists clenched, backpack slammed. No injuries.A: Peer conflict escalating to anger response.P: Referral for anger management skills training.**Counselor Service Notes:**30‑minute session.Interventions: breathing techniques, role play, trigger identification.Clinical impression: student benefited, needs reinforcement.Resolution: student agreed to practice techniques and meet again.**Billing:**Claim code: H2014HAU4Units: 30 ÷ 15 = 2Rate: $20.11/unitTotal = $40.22**Security Architecture Walkthrough (PII‑Only Encryption)**

### **1\. Roster Upload (Before Any Reporting)**

- Admin generates a **digitally‑signed, one‑time‑upload link**.
- Link expires after **15 minutes** or after one use.
- Admin uploads roster; encryption occurs locally.
- Backend stores only encrypted roster entries.

### **2\. Incident Reporter Submission**

- Reporter enters full narrative locally.
- PII is redacted and replaced with placeholders.
- System filters and displays relevant students.
- Reporter selects and approves the correct student ID.

### **3\. Counselor Session & Documentation**

- Counselor receives narrative with placeholders along with Encrypted User information.
- Counselor decrypts the information locally to see the full narrative.
- Counselor enters full session details locally.
- PII is redacted and replaced with placeholders.
- Counselor selects student ID from list.

### **4\. Admin Oversight & Billing Integration**

- Admin decrypts PII locally using private key.
- Admin reviews and approves mappings.
- CYBHI billing mappings are always human‑approved.
- Admin requests updates if errors are identified.
- Billing submission includes:
    - CYBHI code
    - Counselor notes
    - Human‑approved student identifiers
    - Billing calculation

### **5\. Compliance**

- All decryptions and mapping approvals logged.
- FERPA/HIPAA compliance maintained.
- Evolo AI staff cannot decrypt Student PII.
- Human‑in‑the‑loop ensures accuracy and prevents mis‑billing.
