# MasterPass - Secure Password Manager

### Authors: Samantha Freeland & Vanessa Benavente
### Course: Applied Cryptography
### Date: December 4, 2024
### Presentation Link: https://vimeo.com/1036148729/bc9bd5a316?share=copy

## Overview
A cryptographically secure password management application designed to safely store and manage service credentials. Implemented with advanced security features:

- **Master Password Protection**: Bcrypt-based salted password hashing
- **Encryption**: AES encryption for service names and passwords
  - Service Names: AES-ECB with PKCS7 padding
  - Passwords: AES-CTR mode with unique nonces
- **Key Derivation**: SHA-512 for generating encryption and HMAC keys
- **Integrity Verification**: HMAC-SHA256 to detect unauthorized modifications
- **Backup Mechanism**: RSA public-key encryption for master password backup

## Prerequisites

### System Requirements
- Python 3.8+
- pip

### Required Libraries
- `cryptography`
- `bcrypt`

## Installation

1. Clone the Repository
```bash
git clone https://github.com/your-username/password-manager.git
cd password-manager
```

2. Create a Virtual Environment (Recommended)
```bash
python3 -m venv venv
source venv/bin/activate  # Unix/macOS
# On Windows: venv\Scripts\activate
```

3. Install Dependencies
```bash
pip3 install -r requirements.txt
```

## Usage

### Running the Application
```bash
python3 password_manager.py
```

### Workflow

1. **Sign Up**
   - Create a master password
   - System generates:
     * Bcrypt-hashed password
     * RSA key pair for backup
     * Encrypted credential storage

2. **Sign In**
   - Enter master password
   - Access password management features:
     * Add new service credentials
     * Retrieve existing credentials
     * Update passwords
     * Delete service credentials

3. **Available Actions**
   - Add a new service
   - Retrieve a service's credentials
   - Update an existing service's password
   - Delete a service's credentials
   - Sign out securely

## Security Features

### Key Security Mechanisms
- Bcrypt for master password protection
- AES encryption with unique nonces
- HMAC for data integrity verification
- Secure key derivation using SHA-512
- Optional RSA private key passphrase

## Security Recommendations

- Use a strong, unique master password
- Store the RSA private key securely offline
- Be cautious of physical and digital security risks
- Avoid sharing credentials or master password
- Regularly update passwords

## Limitations & Considerations
- Credentials stored locally as individual files
- Requires secure local system access
- No cloud synchronization
- Manual key and credential management

## Disclaimer
This is an educational project demonstrating cryptographic principles. Not recommended for production use without further security audits.
