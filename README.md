# Cipher — Client-Side File Encryptor

Cipher is a zero-trust, 100% client-side file encryption web application built using standard browser Web Crypto APIs (**AES-256-GCM** and **PBKDF2**).

## Features

- **End-to-End Local Processing**: Your files and passwords never leave your web browser or local device.
- **AES-256-GCM Authenticated Encryption**: Provides confidentiality along with built-in message integrity checking to prevent tampering or corruption.
- **PBKDF2 Key Derivation**: High-iteration key derivation (`250,000` iterations of `SHA-256` with a `16-byte` cryptographically secure random salt).
- **Drag & Drop Interface**: Easily encrypt and decrypt files of any type and size.
- **Preserved Filenames**: Encrypted `.cipher` containers safely store the original UTF-8 filename inside the container header so it can be perfectly restored upon decryption.
- **Responsive & Modern Design System**: Uses Google Fonts (`Open Sans` & `Merriweather`), dark-themed cards, password strength indicators, and clean control feedback.

---

## Technical Specifications & `.cipher` File Format Layout

The custom binary format produced by Cipher (`.cipher`) contains a compact header followed by the AES-256-GCM ciphertext payload.

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

Since Cipher is a zero-dependency, pure client-side web application, no server installation or build steps are required.

1. Open `index.html` in any modern web browser (Chrome, Edge, Firefox, Safari).
2. Select **Encrypt** or **Decrypt** mode.
3. Drop a file into the dropzone (or click to browse).
4. Enter a strong password.
5. Click **Encrypt File** / **Decrypt File** to process and download the resulting file.
