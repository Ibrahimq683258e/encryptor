# Cipher — Client-Side File Encryptor Website

Cipher is a modern, zero-trust, 100% client-side file encryption web application and landing page built using standard browser Web Crypto APIs (**AES-256-GCM** and **PBKDF2**).

## Features & Website Highlights

- **Complete Web Landing Page**: Includes Navigation, Hero Masthead, Integrated Working Encryptor Tool, Privacy Features Grid, Security Specifications, and Footer using standard `Open Sans` and `Merriweather` typography and styling.
- **End-to-End Local Encryption**: Your files and passwords never leave your browser memory.
- **AES-256-GCM Authenticated Encryption**: Guarantees data confidentiality and prevents file corruption or tampering.
- **PBKDF2 Key Derivation**: High-iteration key derivation (`250,000` iterations of `SHA-256` with a `16-byte` cryptographically random salt).
- **Drag & Drop Uploads**: Process files of any type and size.
- **Output Naming**: Encrypted output files are generated as `filename.ext.cipher` and retain their original UTF-8 filename inside the header for seamless decryption.

---

## Technical Specifications & `.cipher` File Format Layout

```
+---------------------------------------------------------------------------------------------+
| Magic Header  | Version | Salt        | Initialization Vector (IV) | Filename Length | Original Filename | Ciphertext + Auth Tag |
| 4 bytes       | 1 byte  | 16 bytes    | 12 bytes                  | 2 bytes (Uint16)| N bytes (UTF-8)   | Remaining bytes       |
| ('CPHR')      | (0x01)  | (Random)    | (Random)                  | Big-Endian      |                   | (AES-256-GCM)         |
+---------------------------------------------------------------------------------------------+
```

### Format Breakdown:
1. **Magic Header** (`4 bytes`): ASCII `CPHR` (`0x43`, `0x50`, `0x48`, `0x52`)
2. **Version** (`1 byte`): Container format version (`0x01`)
3. **Salt** (`16 bytes`): Cryptographically secure random salt generated via `crypto.getRandomValues()`
4. **IV** (`12 bytes`): Cryptographically secure random 96-bit Initialization Vector for AES-GCM
5. **Filename Length** (`2 bytes`): 16-bit Big-Endian unsigned integer specifying the byte length of the UTF-8 encoded original filename
6. **Original Filename** (`N bytes`): UTF-8 encoded string of the original file name
7. **Encrypted Payload**: AES-256-GCM ciphertext + 16-byte authentication tag

---

## How to Run

1. Open `index.html` in any modern web browser.
2. Select **Encrypt** or **Decrypt** mode in the Cipher tool section.
3. Drop a file into the dropzone (or click to browse).
4. Enter a password.
5. Click **Encrypt file** / **Decrypt file** to process locally and download `filename.ext.cipher` or restored original file.
