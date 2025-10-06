# Final Project: Password Manager

## Project Overview
For the final project of the **Applied Cryptography** course, you will design and implement a fully functional **Password Manager**. The goal of the project is to integrate cryptographic techniques and principles learned throughout the course. Your task is to build a password manager that allows users to securely store and retrieve passwords for various services or websites. The password manager will be protected by a **master password**, ensuring that only authorized users can access stored passwords. The project will focus on **safe encryption**, **key derivation**, **integrity checking**, and **security protocols**.

Additionally, students will implement a **Master Password Recovery** mechanism using **RSA public/private keys**, allowing them to securely back up the master key for recovery in case it is forgotten.

---

## Project Features

### Core Features:
1. **Secure Storage and Retrieval** of passwords for different services/websites.
2. **Master Password Protection** to ensure only authorized users can access stored passwords.
3. **Encryption** of stored passwords using strong cryptographic techniques.
4. **Salting and Hashing** of the master password.
5. **Master Key Backup** and **Recovery** using RSA (public/private key encryption).
6. **Password Management Functionalities**:
   - Add new services and passwords.
   - Retrieve, update, and delete passwords.
7. **HMAC for Integrity**: Ensure integrity of service names and passwords using HMAC.
8. **User Authentication** based on the master password.

### Cryptographic Techniques:
1. **AES-CTR (Advanced Encryption Standard - Counter Mode)** for password encryption.
2. **Scrypt** for key derivation.
3. **SHA-256 or SHA-3** for password hashing.
4. **RSA (for Master Key Backup/Recovery)** for public/private key cryptography.
5. **HMAC (Hash-based Message Authentication Code)** for service and password integrity.

---

## Deliverables

- **Source Code**: Complete implementation of the password manager in Python 3.x.
- **README**: A comprehensive `README.md` explaining installation, setup, and usage.
- **Report**: A detailed report `report.md` explaining the cryptographic methods, design choices, and approaches used.
- **Video Presentation**: A 5-minute recorded presentation demonstrating the functionality of your password manager.

---

## Project Requirements

- Implement the password manager using **Python 3.x**.
- Use the cryptographic libraries covered in class (e.g., `hashlib`, `cryptography`).
- Follow **cryptography best practices** to avoid vulnerabilities: use proper padding, secure ciphers, prevent nonce reuse, etc.
- The system should be designed for a **single user**; no need for multiple usernames.
- Store passwords in a **text file** or use an **SQL database** (optional).
- Implement a **user-friendly interface** (terminal-based) that is intuitive and secure.
- Include **error handling** for cryptographic operations.

---

## Functional Requirements

### 1. **Sign-Up**
   - Receive a **master password** from the user.
   - **Salt** and **hash** the master password using **SHA-256** or **SHA-3**.
   - Store the **salted and hashed password** in `master_pass.txt` without encryption:
     - File content: `salted_hashed_pass`, `salt`.

   - **Master Key Backup** (New RSA Functionality):
     - Upon sign-up, generate an **RSA key pair**.
     - Derive the **master key** from the master password using **Scrypt**.
     - Encrypt the master key using the **RSA public key** and store it in `backup_key.txt`.
     - Store the RSA private key in a secure location (this key will be needed to recover the master password).

### 2. **Sign-In**
   - Prompt the user for the **master password**.
   - **Authenticate the user**:
     - Use the stored salt to hash the input password.
     - Compare the salted hash with the stored hash.
     - If correct, derive the **master key** and the **HMAC key** using **Scrypt** (separately for each key).
     - Allow access if all checks pass.

### 3. **Adding New Services and Passwords**
   - After sign-in, users can **add new services** and their corresponding passwords.
   - Prompt the user to give a service_name and its password.
   - For each new service:
     - Encrypt the **service name** using **AES-ECB** with a key derived from the master password.
     - Ensure that the service name does not already exist.
     - Generate a 128-bit **nonce** for AES-CTR encryption of the password. Encrypt the password using **AES-CTR** and the derived key and this nonce.
     - Generate **HMACs** for both the encrypted service name and the encrypted password using the **HMAC key**.
     - Store the following in `master.txt` (or SQL DB): 
       - `aes-ecb-encrypted(service_name, master_key)`, `aes-ctr-encrypted(password, nonce, master_key)`, `nonce`, `hmac(service_name, hmac_key)`, `hmac(password, hmac_key)`.

### 4. **Retrieving Passwords**
   - Prompt for the **service name**.
   - Encrypt the service name using AES-ECB to find the corresponding record.
   - **Verify the HMAC** for the service name to ensure integrity.
   - **Decrypt the password** for the service using AES-CTR and the nonce for this service.
   - Verify the **HMAC** for the password to ensure integrity.
   - Display the password to the user.

### 5. **Updating/Deleting Passwords**
   - Allow users to **update** or **delete** passwords for existing services.
   - Ensure proper cryptographic operations are handled securely during these actions, including re-computation of HMACs if updated.

### 6. **Changing the Master Password**
   - Implement functionality to allow users to change the **master password**.
   - Re-derive the **master key** and **HMAC key** and re-encrypt all passwords and re-compute HMACs with the new keys.

### 7. **Sign-Out**
   - Ensure proper **cache clearing** of sensitive data (i.e., unload the master key, HMAC key, and other cryptographic material from memory).

### 8. **Master Password Recovery**

- If a user forgets their **master password**, they can recover it using their RSA private key.
  
#### Master Password Recovery Process:
1. **Backup**:
   - As discussed, during sign-up, when the master password is created, generate an RSA key pair.
   - Use **Scrypt** to derive the master key from the master password.
   - Encrypt the master key using the **RSA public key**.
   - Store the encrypted master key in `backup_key.txt` for future recovery.

2. **Recovery**:
   - If the user forgets their master password:
     - Ask user to provide the RSA private key or location of private key file.  
     - Use the **RSA private key** to decrypt the **backup master key** stored in `backup_key.txt`.
     - Show the master password to the user. 

---

## Cryptographic Components

### 1. **Hash Functions**
   - Use a secure cryptographic hash function, such as **SHA-256** or **SHA-3**, to hash and salt passwords.
   - Store only the hashed versions of the passwords to avoid plaintext storage.

### 2. **AES-ECB** and **AES-CTR (Counter Mode)**
   - Use **AES-ECB** to securely encrypt/decrypt service names.
   - Use **AES-CTR** to securely encrypt/decrypt passwords.
   - Ensure proper **key management** and **nonce reuse prevention**.

### 3. **RSA (Public/Private Key Pair for Recovery)**
   - Use **RSA** to manage the backup and recovery of the master key.
   - RSA public key is used to encrypt the master key during sign-up.
   - RSA private key is used to recover the master key in case of a forgotten master password.

### 4. **Key Derivation Using Scrypt**
   - Derive encryption keys from the master password using the **Scrypt** library.
   - Derive two separate keys from the master password: one for encryption and one for HMAC.

### 5. **HMAC (Hash-based Message Authentication Code)**
   - Use HMAC with a different key derived from the master password to ensure the integrity of service names and passwords.
   - HMAC ensures that any tampering with the encrypted data can be detected by verifying the message's authenticity.

---

## Grading Criteria

- **Code Functionality**: Proper implementation of all required features.
  - Be sure to **comment** your code thoroughly to explain cryptographic operations.
  - Ensure the **user experience** is intuitive, even if the interface is terminal-based.
- **Cryptographic Security**: Correct usage of cryptographic primitives (e.g., avoiding weak ciphers, managing nonces properly).
- **Code Quality**: Readability, structure, and proper error handling.
- **README Quality**: Clear, detailed, well-structured and easy-to-follow guidelines for working and testing the program.
- **Report Quality**: Clear explanation of design choices, secure cryptographic methods used, and lessons learned (5-10 lessons with detailed explanation).
