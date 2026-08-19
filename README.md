# Protecting Social Media Accounts from Hacking
## Network Security & Cryptographic Solution Design

> **Semester Assignment** — CIS311: Network Security and Cryptography  
> **Faculty of Science and Information Technology (FSIT)**  
> **Department of Computing and Information System (CIS)**  
> **Daffodil International University**

---

## 📌 Project Overview & Metadata

| Field | Details |
| :--- | :--- |
| **Course Title** | Network Security and Cryptography |
| **Course Code** | CIS311 |
| **Level / Term** | Level 2, Term 3 |
| **Student Name** | Fayek Ahanaf |
| **Student ID** | 252-16-056 |
| **Section** | 20_A |
| **Submitted To** | Sifat Jahan Sorna, Lecturer, Dept. of CIS, Daffodil International University |
| **Submission Date** | 3 August 2026 |

---

## 📋 Executive Summary

Social media accounts store private messaging, photos, address books, personal details, and public reputation. Therefore, an account takeover may result in service abuse, impersonation, harassment, privacy violations, and reputational damage to third parties. 

This project presents a comprehensive, secure design for a realistic evaluation platform named **SecureConnect**. The design is built upon multi-layered authentication, robust cryptography, Public Key Infrastructure (PKI), secure session management, real-time monitoring, and controlled account recovery. It uses a hybrid defense-in-depth approach to eliminate single points of failure.

### Key Cryptographic Suite:
* **Transport Layer Security (TLS 1.3)**: Server authentication and ephemeral traffic encryption.
* **AES-256-GCM (Galois/Counter Mode)**: Authenticated bulk encryption for data-at-rest (databases & media storage).
* **RSA-3072**: PKI operations, certificate validation, and digital signatures.
* **Argon2id**: Memory-hard password hashing with per-account salt.
* **SHA-256 & RSA-PSS**: Digital signatures for high-risk security-event audit records.
* **Phishing-Resistant Passkeys & MFA**: Secure multi-factor user authentication.

---

## 📖 Table of Contents

1. [Abbreviations](#-abbreviations)
2. [Scenario, Scope and Assumptions](#-scenario-scope-and-assumptions)
3. [Task 1: Cryptography Fundamentals in Context](#-task-1-cryptography-fundamentals-in-context)
   - [Q1. CIA Triad Application](#q1-explain-how-the-cia-triad-confidentiality-integrity-availability-applies-to-the-data-and-communications)
   - [Q2. Passive & Active Attacks (OSI Architecture)](#q2-describe-relevant-passive-and-active-security-attacks-and-required-services)
4. [Task 2: Cryptographic Algorithms and Techniques](#-task-2-cryptographic-algorithms-and-techniques)
   - [Q3. Algorithm Comparison (AES-256-GCM vs RSA-3072 vs DES)](#q3-identify-and-analyze-encryption-algorithms)
   - [Q4. RSA Number-Theoretic Basis & Worked Example](#q4-rsa-number-theoretic-basis--worked-numerical-example)
5. [Task 3: PKI and Cryptographic Solutions](#-task-3-pki-and-cryptographic-solutions)
   - [Q5. PKI Component Design & Architecture Lifecycle](#q5-public-key-infrastructure-pki-design)
   - [Q6. Digital Signatures & SHA-256 Hashing Process](#q6-digital-signatures-hashing-and-non-repudiation)
6. [Task 4: Evaluating Risks and Recommending Best Practices](#-task-4-evaluating-risks-and-recommending-best-practices)
   - [Q7. Security, Privacy & Trust Risk Matrix](#q7-evaluate-security-privacy-and-trust-risks)
   - [TLS & IPsec Risk Reduction](#how-tls-and-ipsec-reduce-the-risks)
   - [Recommended Security Practices](#recommended-security-practices)
   - [Q8. Final Justification & Conclusion](#q8-justification-and-conclusion)
7. [References](#-references)
8. [Annex A: AI Use Disclosure Statement](#-annex-a-ai-use-disclosure-statement)

---

## 🔤 Abbreviations

| Term | Meaning |
| :--- | :--- |
| **AES** | Advanced Encryption Standard |
| **CA** | Certificate Authority |
| **CIA** | Confidentiality, Integrity and Availability |
| **CRL** | Certificate Revocation List |
| **ECDHE** | Elliptic-Curve Diffie-Hellman Ephemeral |
| **HSM** | Hardware Security Module |
| **IPsec** | Internet Protocol Security |
| **MFA** | Multi-Factor Authentication |
| **OCSP** | Online Certificate Status Protocol |
| **PKI** | Public Key Infrastructure |
| **RSA** | Rivest-Shamir-Adleman |
| **SIEM** | Security Information and Event Management |
| **TLS** | Transport Layer Security |
| **WAF** | Web Application Firewall |

---

## 🔍 Scenario, Scope and Assumptions

**SecureConnect** is treated as a medium-sized social media platform providing web and mobile applications. Users can create profiles, publish posts, exchange direct messages, and recover accounts when authenticators are lost.

### Focus Scope:
Protection of account credentials, authentication traffic, session tokens, profile data, private messages, and security-event audit logs.

### Core Design Assumption:
> The highest-value target is the user account. Therefore, account recovery, session lifecycle, and administrator access are protected at least as strongly as the normal login process. A secure password alone is not considered sufficient.

---

## 🛡️ Task 1: Cryptography Fundamentals in Context

### Q1. Explain how the CIA Triad (Confidentiality, Integrity, Availability) applies to the data and communications.

| CIA Property | Meaning for SecureConnect | Concrete Failure Example | Main Safeguards |
| :--- | :--- | :--- | :--- |
| **Confidentiality** | Only authorized users and services read private messages, media, contact details, recovery codes, and security settings. | An attacker steals a session cookie and reads private messages without knowing the password. | TLS 1.3; AES-GCM encryption at rest; least-privilege access; HttpOnly/Secure cookies; HSM/KMS keys. |
| **Integrity** | Posts, profile details, login settings, and audit records must not be changed without authorization or detection. | A hijacker changes the registered email address, deletes recovery codes, and posts fraudulent content. | Authenticated encryption; SHA-256 hashes; digital signatures for high-risk events; database access controls; tamper-evident logging. |
| **Availability** | Login, posting, messaging, and recovery services remain accessible to legitimate users. | A botnet floods the login API, or an attacker locks a victim out through repeated failed logins. | Rate limiting; DDoS protection (WAF/CDN); load balancing; redundant services; tested backups & disaster recovery. |

---

### Q2. Describe relevant passive and active security attacks and required services (OSI Security Architecture X.800).

| Type | Attack | Social-Media Example | Required Service | Mechanisms |
| :--- | :--- | :--- | :--- | :--- |
| **Passive** | Eavesdropping | Sniffing open Wi-Fi to capture login requests, messages, or session tokens. | Data Confidentiality; Authentication | TLS 1.3 encryption; certificate validation; secure cookies; optional traffic padding. |
| **Passive** | Traffic Analysis | Observing login frequencies, message sizes, or activity patterns. | Traffic-Flow Confidentiality | TLS record padding; CDN/proxy aggregation; minimal metadata retention. |
| **Active** | Masquerade / Phishing | Fake login pages collecting credentials or using stolen credentials to impersonate a victim. | Peer-Entity Authentication; Access Control | Passkeys / MFA; server X.509 certificates; risk-based login checks; device alerts. |
| **Active** | Replay Attack | Replaying captured reset tokens, API requests, or auth responses. | Data-Origin Authentication; Integrity | Nonces; timestamps; short-lived one-time tokens; TLS anti-replay controls. |
| **Active** | Message Modification / MITM | Altering profile updates or redirecting clients to impostor services. | Integrity; Authentication | TLS 1.3 AEAD; X.509 certificates; HSTS; signed high-risk events. |
| **Active** | Session Hijacking | Stolen session cookies used to hijack account state post-login. | Access Control; Integrity; Authentication | HttpOnly/Secure/SameSite cookies; token rotation; re-authentication for sensitive actions. |
| **Active** | Denial of Service (DoS) | Flooding login/recovery endpoints to exhaust system resources. | Availability | WAF/CDN; rate-limiting queues; autoscaling; circuit breakers; safe recovery priority. |

---

## ⚡ Task 2: Cryptographic Algorithms and Techniques

### Q3. Identify and analyze encryption algorithms (AES-256-GCM vs RSA-3072 vs DES).

| Algorithm | Type | Key / Block Size | Structure | Strengths | Limitations | Use in Proposal |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **AES-256-GCM** | Symmetric Block Cipher (AEAD) | 256-bit key; 128-bit block | Substitution-Permutation Network (SPN) + Polynomial Auth Tag | Fast in hardware/software; confidentiality & integrity in one operation | Nonce reuse under same key is dangerous; requires key rotation | Primary bulk-data & data-at-rest algorithm (NIST standard). |
| **RSA-3072** | Asymmetric Public-Key Algorithm | 3072-bit modulus; separate keys | Modular Exponentiation (Factoring composite integers) | Supports certificates, digital signatures, public distribution | Slower than AES; large ciphertext/signatures; quantum vulnerable | Used for X.509 certificates and RSA-PSS signatures (RFC 8017). |
| **DES** *(Rejected)* | Symmetric Block Cipher | 56-bit key; 64-bit block | 16-round Feistel Network | Historically important; simple structure | 56-bit key is vulnerable to brute-force; small block size collision risk | **Rejected**. Included only to justify modern algorithm necessity. |

#### Hybrid Cryptography Workflow:
```
[X.509 Certificate] ---> [Ephemeral ECDHE Handshake] ---> [HKDF Key Derivation] ---> [AES-256-GCM Encrypted Session]
(Authenticates Server)     (Provides Perfect Forward Secrecy)  (Derives TLS 1.3 Keys)   (Encrypts Login & API Traffic)
```

---

### Q4. RSA Number-Theoretic Basis & Worked Numerical Example

RSA relies on the mathematical difficulty of factoring the product of two large prime numbers.

#### Worked Demonstration (Small Primes Example):
1. **Choose Primes**: $p = 61$, $q = 53$
2. **Compute Modulus ($n$)**: $n = p \times q = 61 \times 53 = 3233$
3. **Compute Euler's Totient ($\phi(n)$)**: $\phi(n) = (p - 1)(q - 1) = 60 \times 52 = 3120$
4. **Select Public Exponent ($e$)**: Choose $e = 17$, where $\gcd(17, 3120) = 1$
5. **Calculate Private Exponent ($d$)**: Solve $e \times d \equiv 1 \pmod{3120} \implies d = 2753$  
   *(Check: $17 \times 2753 = 46801 = 15 \times 3120 + 1$)*
6. **Form Keys**:  
   * **Public Key**: $(e, n) = (17, 3233)$  
   * **Private Key**: $(d, n) = (2753, 3233)$

#### Encryption & Decryption Simulation:
* **Plaintext message**: $m = 65$
* **Encryption Formula**: $c = m^e \pmod{n} = 65^{17} \pmod{3233}$
  * $65^2 \pmod{3233} = 992$
  * $65^4 \pmod{3233} = 1232$
  * $65^8 \pmod{3233} = 1547$
  * $65^{16} \pmod{3233} = 789$
  * $c = (65^{16} \times 65) \pmod{3233} = (789 \times 65) \pmod{3233} = \mathbf{2790}$
* **Decryption Formula**: $m = c^d \pmod{n} = 2790^{2753} \pmod{3233} = \mathbf{65}$

---

## 🔐 Task 3: PKI and Cryptographic Solutions

### Q5. Public Key Infrastructure (PKI) Design

| PKI Component | Role in SecureConnect Platform |
| :--- | :--- |
| **Offline Root CA** | Top trust anchor. Signs only intermediate CA certificates; kept offline in secure facility. |
| **Intermediate / Issuing CA** | Issues short-lived certificates to web servers and internal microservices. |
| **Registration Authority (RA)** | Verifies service identity, domain control, and authorization before certificate issuance. |
| **Certificate Repository** | Publishes certificate chains for client path validation. |
| **OCSP / CRL Service** | Provides real-time certificate status and revocation checking. |
| **HSM / KMS** | Hardware protection for private keys, signing operations, and automated key rotation. |
| **Relying Parties** | Browsers, mobile apps, API gateways validating certificates. |
| **Audit / SIEM** | Collects key usage, certificate issuance, and validation failure events. |

#### Architecture Lifecycle & Authentication Flow:
1. User accesses SecureConnect via HTTPS.
2. Server presents X.509 certificate chain (validated per RFC 5280).
3. TLS 1.3 establishes key exchange and encrypts channel traffic.
4. User authenticates via **Passkey** or **Password + MFA App (Argon2id hashing)**.
5. Server issues a `Secure`, `HttpOnly`, `SameSite` session cookie (no sensitive PII inside token).
6. Sensitive operations require step-up re-authentication.

---

### Q6. Digital Signatures, Hashing, and Non-Repudiation

To guarantee integrity and non-repudiation for high-risk account events (e.g., recovery approvals or passkey changes), digital signatures are applied:

#### Worked Hashing Example:
* **Canonical Record**: `"User U1042 enabled passkey at 2026-07-31T14:20:00Z"`
* **SHA-256 Digest**: `0460d913018c9b39414bea60480f3b40bff39d53c3af7e7102216f7673442619`
* **Signing**: HSM signs SHA-256 digest using **RSA-PSS** private key.
* **Verification**: Public key recalculates digest and validates RSA-PSS signature.

---

## 📈 Task 4: Evaluating Risks and Recommending Best Practices

### Q7. Evaluate Security, Privacy, and Trust Risks

| Risk | Likelihood | Impact | Main Mitigation | Residual Risk |
| :--- | :--- | :--- | :--- | :--- |
| **Credential Stuffing** | High | High | Passkeys/MFA, rate limiting, breached-password screening (OWASP). | Medium |
| **Phishing / Fake Support** | High | High | Phishing-resistant passkeys, mandatory step-up checks, dual control. | Medium |
| **Session Hijacking** | Medium | High | Secure/HttpOnly/SameSite cookies, CSP, session token rotation. | Medium |
| **MITM / Cert Compromise** | Low-Medium | High | TLS 1.3, HSTS, short-lived certs, HSM private keys, OCSP/CRL. | Low-Medium |
| **Database Breach** | Medium | High | AES-256-GCM data encryption, Argon2id password hashing with salt. | Medium |
| **DDoS Attacks** | High | Medium-High | CDN/WAF, autoscaling, adaptive rate-limiting, CAPTCHA on anomaly. | Medium |

---

### How TLS and IPsec Reduce the Risks

* **TLS 1.3**: Provides application-layer protection (server authentication, confidentiality, record integrity) mitigating eavesdropping, credential sniffing, and MITM.
* **IPsec**: Applied at network layer (tunnel mode) for site-to-site VPNs, database replication, and admin management connections across data centers.

---

### Recommended Security Practices
* Prefer **passkeys** over SMS-based MFA to prevent SIM-swapping.
* Protect long-lived passwords with **Argon2id** memory-hard hashing (RFC 9106).
* Enforce strict **session token rotation** upon privilege changes.
* Maintain private keys inside a dedicated **Hardware Security Module (HSM)**.
* Develop an actionable **Account Compromise Response Plan** and cryptographic migration strategy.

---

### Q8. Justification and Conclusion

The proposed **SecureConnect** solution represents a robust, trustworthy, and responsible application of network security principles:
* **Robust**: Adopts a defense-in-depth model combining TLS 1.3, AES-256-GCM, RSA-3072, and Argon2id.
* **Trustworthy**: Eliminates single points of failure, guarantees data integrity through RSA-PSS digital signatures, and ensures privacy through data minimization.
* **Responsible**: Aligns with global standards (NIST SP 800-63B, FIPS 186-5, OWASP guidelines, RFC 9846, RFC 9106).

---

## 📚 References

1. ITU-T, *"ITU-T X.800: Security Architecture for Open Systems Interconnection"*, 1991.
2. NIST, *"FIPS 197: Advanced Encryption Standard (AES)"*, updated 2023.
3. K. Moriarty et al., *"RFC 8017: PKCS #1: RSA Cryptography Specifications Version 2.2"*, 2016.
4. E. Rescorla, *"RFC 9846: The Transport Layer Security (TLS) Protocol Version 1.3"*, July 2026.
5. D. Cooper et al., *"RFC 5280: Internet X.509 Public Key Infrastructure Certificate and CRL Profile"*, 2008.
6. NIST, *"FIPS 180-4: Secure Hash Standard (SHS)"*, 2015.
7. NIST, *"FIPS 186-5: Digital Signature Standard (DSS)"*, 2023.
8. D. Temoshok et al., *"NIST SP 800-63B-4: Digital Identity Guidelines"*, NIST, 2025.
9. A. Biryukov et al., *"RFC 9106: Argon2 Memory-Hard Function for Password Hashing"*, 2021.
10. OWASP Foundation, *"Authentication & Session Management Cheat Sheets"*, OWASP Series.

---

## 🤖 Annex A: AI Use Disclosure Statement

* **Tool Used**: ChatGPT (GPT-5.5 / OpenAI)
* **Purpose**: Advisory assistance for structuring academic layout, refining grammar clarity, formatting markdown tables, and organizing cryptographic architecture explanations.
* **Declaration**: All analytical decisions, cryptographic calculations, architectural choices, and report contents were independently reviewed, verified, and validated by the author.

---
*Created by **Fayek Ahanaf** (Student ID: 252-16-056) — Daffodil International University*
