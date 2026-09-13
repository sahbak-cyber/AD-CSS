<h1>Active Directory Certificate Services (AD CS) & EFS Project</h1>

<h3>Internal Public Key Infrastructure (PKI), Certificate Enrolment, and Encrypting File System (EFS) Implementation</h3>

<p>
This project demonstrates the design and implementation of an internal Public Key Infrastructure (PKI) using Microsoft Active Directory Certificate Services (AD CS) on Windows Server 2022.
The lab was built within the simulated <b>Sahbak</b> Active Directory environment and demonstrates how digital certificates, public/private key cryptography, Active Directory identities, and the Windows Encrypting File System (EFS) can work together to protect sensitive data.
</p>

<p>
An Enterprise Root Certificate Authority was deployed within the <b>sahbak.com</b> domain using Active Directory Certificate Services.
The Certificate Authority, <b>SAHBAK.COM-CA</b>, was configured as the internal trust anchor responsible for issuing and signing certificates for authorised domain identities.
</p>

<p>
A domain-joined Windows 8.1 workstation named <b>AuditPC1</b> was used as the EFS testing endpoint.
Two domain users were used to demonstrate authorised and unauthorised access:
</p>

<ul>
  <li><b>Borice Kolley</b> – authorised EFS certificate holder and file owner</li>
  <li><b>Bola Bakinson</b> – valid domain user without the private key required to decrypt the protected file</li>
</ul>

<h2>Lab Environment</h2>

<ul>
  <li>Windows Server 2022</li>
  <li>Active Directory Domain Services (AD DS)</li>
  <li>Active Directory Certificate Services (AD CS)</li>
  <li>DNS</li>
  <li>Domain: <b>sahbak.com</b></li>
  <li>Enterprise Root CA: <b>SAHBAK.COM-CA</b></li>
  <li>Windows 8.1 Pro domain-joined client: <b>AuditPC1</b></li>
  <li>Encrypting File System (EFS)</li>
</ul>

<h2>Project Objectives</h2>

<ul>
  <li>Deploy an internal Enterprise Root Certificate Authority using AD CS</li>
  <li>Establish an internal PKI trust foundation within Active Directory</li>
  <li>Configure and use an EFS-capable certificate template</li>
  <li>Enrol an authorised domain user for an EFS certificate</li>
  <li>Use the certificate to encrypt sensitive data using EFS</li>
  <li>Verify certificate issuance and private-key availability</li>
  <li>Validate file encryption using Windows encryption tools</li>
  <li>Test access using an unauthorised domain user</li>
  <li>Demonstrate the relationship between identity, certificates, cryptography, and access control</li>
</ul>

<h2>AD CS Implementation</h2>

<p>
The Active Directory Certificate Services role was installed on Windows Server 2022 through Server Manager.
The server was configured as an <b>Enterprise Certificate Authority</b> and a <b>Root CA</b>, allowing the Certificate Authority to integrate directly with Active Directory.
</p>

<p>
The Enterprise CA configuration enables the Certificate Authority to use Active Directory identities and certificate templates when determining which users or computers are permitted to enrol for certificates.
The CA acts as the root of trust for certificates issued within the Sahbak lab environment.
</p>

<h2>Public Key Infrastructure (PKI)</h2>

<p>
The project demonstrates the core components of Public Key Infrastructure.
PKI provides the policies, technologies, identities, certificates, and cryptographic keys required to establish digital trust.
</p>

<ul>
  <li><b>Certificate Authority (CA):</b> Issues and digitally signs trusted certificates</li>
  <li><b>Certificate:</b> Binds an identity to a public key</li>
  <li><b>Public Key:</b> Can be shared and is used as part of encryption and signature verification</li>
  <li><b>Private Key:</b> Remains protected by the certificate holder and is required for cryptographic operations such as EFS decryption</li>
  <li><b>Trust Chain:</b> Establishes confidence in certificates issued by a trusted Certificate Authority</li>
</ul>

<h2>Certificate Enrolment</h2>

<p>
An EFS-capable certificate was issued to the authorised domain user <b>Borice Kolley</b>.
Active Directory provided the identity information required for certificate enrolment, while AD CS issued a trusted certificate associated with the user's public/private key pair.
</p>

<p>
The certificate and associated private key were stored within the authorised user's profile on the domain-joined workstation.
This created the cryptographic identity required for EFS file protection.
</p>

<h2>Encrypting File System (EFS)</h2>

<p>
Windows Encrypting File System (EFS) was used to provide file-level encryption on the NTFS file system.
A confidential test file was created and encrypted by the authorised user.
</p>

<p>
When EFS encryption is enabled, Windows generates a random symmetric <b>File Encryption Key (FEK)</b> to encrypt the contents of the file.
The FEK is then protected using information associated with the authorised user's EFS certificate.
The corresponding private key is required to recover the FEK and decrypt the protected file.
</p>

<h2>Authorised Access Test</h2>

<p>
The authorised user <b>Borice Kolley</b> was able to access the EFS-encrypted file because the user's profile contained the EFS certificate and the corresponding private key required to recover the File Encryption Key.
</p>

<p>
For the authorised user, encryption and decryption occurred transparently through Windows while the file remained cryptographically protected on disk.
</p>

<h2>Unauthorised Access Test</h2>

<p>
A second domain account, <b>KOla Abiola</b>, was used to test access to the encrypted file.
Although Bola Bakinson was a valid authenticated user within the <b>sahbak.com</b> domain, the account did not possess the private key corresponding to Borice Kolley's EFS certificate.
</p>

<p>
As a result, EFS could not recover the encrypted File Encryption Key required to decrypt the file contents, demonstrating that successful Active Directory authentication alone does not provide access to EFS-protected data.
</p>

<h2>EFS vs NTFS Permissions</h2>

<p>
The project also demonstrates the distinction between traditional NTFS access control and cryptographic file protection.
</p>

<ul>
  <li><b>NTFS permissions</b> control which identities are authorised to access files and folders through Access Control Lists (ACLs)</li>
  <li><b>EFS</b> protects the actual contents of the file using cryptography</li>
  <li>NTFS ownership or administrative permissions do not automatically provide the private key required to decrypt EFS-protected data</li>
  <li>An authorised EFS certificate/private key or configured recovery mechanism is required to recover protected data</li>
</ul>

<h2>How AD DS, AD CS, and EFS Work Together</h2>

<ol>
  <li>Active Directory provides the user's trusted domain identity</li>
  <li>AD CS validates the identity and issues an EFS-capable certificate</li>
  <li>The certificate binds the user's identity to a public key</li>
  <li>The corresponding private key remains protected within the user's profile</li>
  <li>EFS generates a File Encryption Key to encrypt the file contents</li>
  <li>The FEK is protected using the authorised user's EFS certificate</li>
  <li>The authorised user's private key allows EFS to recover the FEK and decrypt the file</li>
  <li>Users without the required private key cannot decrypt the protected data</li>
</ol>

<h2>Security Principles Demonstrated</h2>

<ul>
  <li>Public Key Infrastructure (PKI)</li>
  <li>Certificate-Based Trust</li>
  <li>Public / Private Key Cryptography</li>
  <li>Identity Binding</li>
  <li>Data Confidentiality</li>
  <li>Cryptographic Access Control</li>
  <li>Certificate Enrolment</li>
  <li>Least Privilege</li>
  <li>Separation of Authentication and Data Decryption Rights</li>
  <li>Defence in Depth</li>
</ul>

<h2>Tools and Technologies</h2>

<ul>
  <li>Windows Server 2022</li>
  <li>Active Directory Domain Services (AD DS)</li>
  <li>Active Directory Certificate Services (AD CS)</li>
  <li>Certificate Authority Management Console</li>
  <li>Certificate Templates</li>
  <li>Microsoft Management Console (MMC)</li>
  <li>Windows 8.1 Pro</li>
  <li>Encrypting File System (EFS)</li>
  <li>NTFS</li>
  <li>Windows Cipher Utility</li>
  <li>Active Directory Users and Computers (ADUC)</li>
</ul>

<h2>Verification and Testing</h2>

<p>
The implementation was validated by confirming that the Certificate Authority was operational, the EFS certificate was successfully issued to the authorised user, the certificate was present within the user's certificate store, and the test file was successfully encrypted.
</p>

<p>
Encryption status was also verified using Windows encryption tools, and access testing confirmed that the authorised certificate holder could decrypt the file while the second domain user could not.
</p>

<h2>Key Learning Outcomes</h2>

<p>
This project provided practical experience in deploying and administering an internal Microsoft Public Key Infrastructure using Active Directory Certificate Services.
It strengthened understanding of Certificate Authorities, Enterprise PKI, certificate templates, certificate enrolment, public/private key pairs, trust relationships, and certificate-based security.
</p>

<p>
The EFS implementation demonstrated how cryptographic protection can complement traditional Active Directory and NTFS access controls.
The project showed that identity authentication, file permissions, certificate ownership, and possession of the correct private key are separate but closely connected security concepts.
</p>

<p>
The project also provided practical insight into how enterprise environments can use Active Directory identities and internal PKI services to provide trusted certificate-based security for users, devices, and protected organisational data.
</p>

<h2>Recommended Next Phase</h2>

<ul>
  <li>Configure an EFS Data Recovery Agent (DRA)</li>
  <li>Implement certificate auto-enrolment through Group Policy</li>
  <li>Test certificate revocation and Certificate Revocation Lists (CRLs)</li>
  <li>Implement certificate renewal and lifecycle management</li>
  <li>Export and securely back up EFS certificates and private keys</li>
  <li>Expand the PKI to support additional certificate use cases such as TLS, device authentication, and smart-card authentication</li>
</ul>
