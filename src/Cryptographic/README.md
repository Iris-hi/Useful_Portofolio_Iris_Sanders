# Cryptographic Module

A comprehensive cryptographic library implementing enterprise-grade encryption and hashing algorithms.

## 🔐 Features

- **AES-256-CBC Encryption/Decryption** - Industry-standard symmetric encryption
- **SHA256 Hashing** - Secure one-way hash function for data integrity
- **HMAC-SHA256** - Message authentication codes for verifying data authenticity
- **Secure Random Generation** - Cryptographically secure random key and IV generation
- **Utility Functions** - Hex/Base64 encoding, string/byte conversions
- **High-level API** - Simple password-based encryption for ease of use
- **Low-level API** - Full control over keys, IVs, and encryption parameters

## 🚀 Quick Start

### Basic String Encryption

```luau
local Crypto = require(script.Cryptographic)

-- Simple password-based encryption
local plaintext = "Hello, World!"
local password = "mySecurePassword"

local encrypted, err = Crypto.encryptString(plaintext, password)
if not err then
    print("Encrypted:", encrypted)

    local decrypted, err2 = Crypto.decryptString(encrypted, password)
    print("Decrypted:", decrypted) -- "Hello, World!"
end
```

### SHA256 Hashing

```luau
local hash = Crypto.sha256Hex("hello world")
print(hash) -- b94d27b9934d3e08a52e52d7da7dabfac484efe37a5380ee9088f7ace2efcde9
```

### Manual AES Encryption

```luau
-- Generate secure key and IV
local key = Crypto.generateAESKey() -- 32 bytes for AES-256
local iv = Crypto.generateIV()      -- 16 bytes

-- Encrypt with custom parameters
local ciphertext, usedIV, err = Crypto.aesEncrypt("secret data", key, iv)
local plaintext, err2 = Crypto.aesDecrypt(ciphertext, key, usedIV)
```

### Message Authentication

```luau
-- Verify message integrity with HMAC
local hmac = Crypto.hmacSha256("secret_key", "important message")
local isValid = Crypto.bytesToHex(hmac) == expected_hmac
```

## 📖 API Reference

### High-Level Functions

#### `encryptString(plaintext: string, password: string) -> (string?, string?)`

Encrypts a string using password-based encryption (AES-256-CBC with SHA256 key derivation).

**Parameters:**

- `plaintext` - The string to encrypt
- `password` - Password for encryption

**Returns:**

- `encrypted` - Base64-encoded encrypted data (includes IV)
- `error` - Error message if encryption failed

#### `decryptString(encrypted: string, password: string) -> (string?, string?)`

Decrypts a string encrypted with `encryptString`.

**Parameters:**

- `encrypted` - Base64-encoded encrypted data
- `password` - Password for decryption

**Returns:**

- `plaintext` - Decrypted string
- `error` - Error message if decryption failed

#### `sha256Hex(data: string | {number}) -> string`

Computes SHA256 hash and returns as hexadecimal string.

**Parameters:**

- `data` - String or byte array to hash

**Returns:**

- Hexadecimal hash string

### Low-Level Functions

#### `aesEncrypt(plaintext: string | {number}, key: {number}, iv?: {number}) -> ({number}?, {number}?, string?)`

AES-256-CBC encryption with manual key/IV control.

**Parameters:**

- `plaintext` - String or byte array to encrypt
- `key` - 32-byte encryption key
- `iv` - 16-byte initialisation vector (auto-generated if nil)

**Returns:**

- `ciphertext` - Encrypted byte array
- `usedIV` - IV used for encryption
- `error` - Error message if encryption failed

#### `aesDecrypt(ciphertext: string | {number}, key: {number}, iv: {number}) -> ({number}?, string?)`

AES-256-CBC decryption with manual key/IV control.

**Parameters:**

- `ciphertext` - String or byte array to decrypt
- `key` - 32-byte decryption key
- `iv` - 16-byte initialisation vector

**Returns:**

- `plaintext` - Decrypted byte array
- `error` - Error message if decryption failed

#### `sha256(data: string | {number}) -> {number}`

Computes SHA256 hash and returns as byte array.

**Parameters:**

- `data` - String or byte array to hash

**Returns:**

- 32-byte hash array

#### `hmacSha256(key: string | {number}, message: string | {number}) -> {number}`

Computes HMAC-SHA256 for message authentication.

**Parameters:**

- `key` - Secret key for authentication
- `message` - Message to authenticate

**Returns:**

- 32-byte HMAC array

### Key Generation

#### `generateAESKey() -> {number}`

Generates a cryptographically secure 32-byte AES-256 key.

**Returns:**

- 32-byte random key array

#### `generateIV() -> {number}`

Generates a cryptographically secure 16-byte initialisation vector.

**Returns:**

- 16-byte random IV array

### Utility Functions

#### `bytesToHex(bytes: {number}) -> string`

Converts byte array to hexadecimal string.

#### `hexToBytes(hex: string) -> ({number}?, string?)`

Converts hexadecimal string to byte array.

#### `bytesToBase64(bytes: {number}) -> string`

Converts byte array to Base64 string.

#### `base64ToBytes(b64: string) -> {number}`

Converts Base64 string to byte array.

#### `stringToBytes(str: string) -> {number}`

Converts string to byte array.

#### `bytesToString(bytes: {number}) -> string`

Converts byte array to string.

## 🔧 Implementation Details

### Security Features

- **Constant-time operations** - Resistant to timing attacks
- **Secure padding** - PKCS7 padding with validation
- **Authenticated encryption** - CBC mode with proper IV handling
- **Key derivation** - SHA256-based password-to-key derivation
- **Secure random generation** - Cryptographically secure randomness

### Standards Compliance

- **AES-256** - FIPS 197 compliant
- **SHA256** - FIPS 180-4 compliant
- **HMAC** - RFC 2104 compliant
- **CBC Mode** - NIST SP 800-38A compliant

### Algorithm Details

#### AES-256-CBC

- **Block Size**: 128 bits (16 bytes)
- **Key Size**: 256 bits (32 bytes)
- **Rounds**: 14
- **Mode**: Cipher Block Chaining (CBC)
- **Padding**: PKCS7

#### SHA256

- **Output Size**: 256 bits (32 bytes)
- **Block Size**: 512 bits (64 bytes)
- **Iterations**: 64 rounds per block
- **Constants**: SHA-2 family constants

#### HMAC-SHA256

- **Hash Function**: SHA256
- **Key Size**: Any (padded to 64 bytes)
- **Output Size**: 256 bits (32 bytes)
- **Standard**: RFC 2104

## 🧪 Examples

See `Example.luau` for comprehensive usage examples including:

- Basic encryption/decryption workflows
- Key generation and management
- HMAC message authentication
- Utility function demonstrations

## ⚠️ Security Considerations

### Key Management

- **Never hardcode keys** in source code
- **Store keys securely** using appropriate key management systems
- **Rotate keys regularly** according to security policies
- **Use secure key derivation** for password-based encryption

### Best Practices

- **Use strong passwords** with sufficient entropy
- **Never reuse IVs** with the same key
- **Validate all inputs** and check for errors
- **Use HMAC** for message authentication when needed
- **Implement proper random generation** for production use

### Attack Resistance

- **Timing attacks**: Constant-time comparisons implemented
- **Padding oracle attacks**: Secure PKCS7 padding validation
- **IV reuse**: Each encryption generates unique IV
- **Weak randomness**: Uses cryptographically secure random generation

## 🏗️ Architecture

```
CryptoModule.luau
├── Utility Functions
│   ├── String/Byte conversions
│   ├── Encoding (Hex/Base64)
│   └── Secure comparisons
├── SHA256 Implementation
│   ├── Message scheduling
│   ├── Compression function
│   └── Padding and processing
├── AES Implementation
│   ├── Key expansion
│   ├── S-box transformations
│   ├── Cipher operations
│   └── CBC mode handling
└── Public API
    ├── High-level functions
    ├── Low-level functions
    └── Utility exports
```

## 📋 Error Handling

All functions return errors as the last return value. Always check for errors:

```luau
local result, error = Crypto.someFunction(...)
if error then
    print("Error:", error)
    return
end
-- Use result safely
```

Common error types:

- Invalid key/IV lengths
- Padding validation failures
- Invalid hex/base64 encoding
- Insufficient data length

## 🧪 Testing

The module includes extensive internal validation and the `Example.luau` file serves as both documentation and basic testing. For production use, consider adding:

- Unit tests for all functions
- Property-based testing for crypto operations
- Security testing against known vectors
- Performance benchmarking

## 📜 License

Licensed under the MIT License - see the main repository LICENSE file for details.

## 👤 Author

**Iris Sanders**

---

_Built from scratch with security and standards compliance in mind_ 🔐
