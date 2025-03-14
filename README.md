
## 📜 Certificate Generation Guide

This guide explains how to generate and prepare the required certificates for signing Apple Wallet `.pkpass` files.

### ✅ 1. Create a Pass Type ID (Apple Developer Portal)

1. Go to [Apple Developer Portal](https://developer.apple.com/account/)
2. Navigate to **Certificates, Identifiers & Profiles**
3. Under **Identifiers**, click **+** to create a new **Pass Type ID**
4. Example: `pass.com.yourcompany.giftcard`
5. Assign your **Team ID**
6. Register the identifier

### ✅ 2. Create a Certificate Signing Request (CSR)

1. Open **Keychain Access** on your Mac
2. From menu: `Keychain Access → Certificate Assistant → Request a Certificate from a Certificate Authority`
3. Fill in:
   - **User Email Address**: your email
   - **Common Name**: e.g., "Apple Wallet Pass"
   - **CA Email**: leave blank
   - **Request is**: Save to disk
4. Click **Continue**, save the `.certSigningRequest` file

### ✅ 3. Generate Pass Certificate (from CSR)

1. Go back to **Apple Developer → Certificates**
2. Click **+** to create a new **Pass Type ID Certificate**
3. Select **Pass Type ID Certificate**
4. Choose the **Pass Type ID** you created
5. Upload the `.certSigningRequest` file
6. Apple will provide a certificate: `PassCertificate.cer`

### ✅ 4. Export Certificate and Private Key to `.p12`

1. Double-click `PassCertificate.cer` to install it in **Keychain Access**
2. In Keychain Access:
   - Locate your certificate under **My Certificates**
   - Right-click → **Export**
   - Save as `Certificates.p12`
   - Choose a strong password (needed in `.env` later)

### ✅ 5. Convert `.p12` to `.pem` files

Use terminal:

```bash
# Convert certificate
openssl pkcs12 -in Certificates.p12 -clcerts -nokeys -out signerCert.pem -passin pass:<your-password> -legacy

# Convert private key
openssl pkcs12 -in Certificates.p12 -nocerts -out signerKey.pem -passin pass:<your-password> -passout pass:<secret-passphrase> -legacy
```

Place files in your project:

```
certs/
├── signerCert.pem
├── signerKey.pem
```

### ✅ 5. Download and Convert Apple WWDR Certificate

1. Download Apple WWDR Certificate (G4): [Apple WWDR](https://www.apple.com/certificateauthority/AppleWWDRCAG4.cer)
2. Convert to PEM:

```bash
openssl x509 -in AppleWWDRCAG4.cer -inform DER -outform PEM -out wwdr.pem
```

Place in your project:

```
certs/
├── signerCert.pem
├── signerKey.pem
└── wwdr.pem
```
