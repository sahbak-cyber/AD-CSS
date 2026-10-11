<h1>🔐 On-Premises AD CS & EFS File Encryption Project</h1>

<h3>Enterprise PKI, Certificate Enrolment and Cryptographic File Protection</h3>

<p>
  <img src="https://img.shields.io/badge/Windows%20Server-2022-0078D4" alt="Windows Server 2022">
  <img src="https://img.shields.io/badge/Active%20Directory-AD%20DS-005A9C" alt="Active Directory">
  <img src="https://img.shields.io/badge/Certificate%20Services-AD%20CS-6A1B9A" alt="AD CS">
  <img src="https://img.shields.io/badge/CA-Enterprise%20Root-FF6600" alt="Enterprise Root CA">
  <img src="https://img.shields.io/badge/Encryption-EFS-008000" alt="EFS">
  <img src="https://img.shields.io/badge/Cryptography-RSA%202048%20%7C%20SHA--256-D71920" alt="RSA 2048 and SHA-256">
</p>

## 📌 Project Overview

This project demonstrates the deployment of an internal Public Key Infrastructure using Microsoft Active Directory Certificate Services on Windows Server 2022.

The project was built within the simulated **Sahbak Active Directory environment** to demonstrate how domain identities, digital certificates, public and private keys, and the Windows Encrypting File System work together to protect sensitive files.

An **Enterprise Root Certification Authority** was deployed and integrated with the `SAHBAK.COM` domain. The **Basic EFS** certificate template was published, an EFS certificate was enrolled for an authorised domain user, and a confidential test file was encrypted on a domain-joined Windows client.

Access was then tested using a second authenticated domain user who did not possess the authorised user’s EFS certificate and private key.

> **Scope:** This is an educational laboratory project demonstrating foundational PKI, certificate-management and EFS concepts. It is not presented as a production PKI design or a claim of regulatory compliance.

---

## 🎯 Project Objectives

- Deploy an Enterprise Root CA using Active Directory Certificate Services.
- Establish an internal PKI trust foundation.
- Integrate the Certification Authority with Active Directory.
- Publish an EFS-capable certificate template.
- Enrol an EFS certificate for an authorised domain user.
- Encrypt a confidential test file using Windows EFS.
- Verify the certificate authorised to decrypt the file.
- Confirm that another authenticated user cannot decrypt the file.
- Demonstrate the relationship between identity, certificates, cryptographic keys and access control.
- Document the implementation and validation process.

---

## 🧪 Lab Environment

Exact server IP addresses and machine-generated hostnames have been removed from this public documentation.

| Component | Configuration |
|---|---|
| Organisation | Sahbak |
| Active Directory domain | `SAHBAK.COM` |
| Certification Authority | `SAHBAK.COM-CA` |
| CA server | Internal Windows Server 2022 server |
| Server IP address | Redacted |
| Server operating system | Windows Server 2022 |
| CA type | Enterprise Root CA |
| Cryptography | RSA 2048-bit key with SHA-256 |
| Certificate template | Basic EFS |
| Client operating system | Windows 8.1 Pro |
| Client device | Domain-joined workstation |
| Test file | `C:\EFS-Test\Confidential.txt` |

---

## 👥 Test Identities

| User | Role in the Test |
|---|---|
| **Borice Kolley** | Authorised EFS certificate holder and encrypted-file owner |
| **Kola Abiola** | Authenticated domain user without Borice Kolley’s EFS private key |

The identities used in this project were created specifically for the simulated laboratory environment.

---

## 🏗️ Architecture

```mermaid
flowchart TD
    AD["Active Directory Domain<br/>SAHBAK.COM"]
    CA["Enterprise Root CA<br/>SAHBAK.COM-CA"]
    EFS["Basic EFS Certificate"]
    Owner["Borice Kolley<br/>Authorised User"]
    File["EFS-Encrypted File<br/>Confidential.txt"]
    Tester["Kola Abiola<br/>Unauthorised User"]

    AD --> CA
    CA --> EFS
    EFS --> Owner
    Owner --> File
    Tester -->|"Decryption denied"| File
