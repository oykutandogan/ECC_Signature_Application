# ECC (Elliptic Curve Cryptography) Signature Application

This project is a Python application that performs message signing and signature verification using **Elliptic Curve Cryptography (ECC)**.

The application offers an easy-to-use interface with **Tkinter**.

---

## 📌 Project Features

- **ECC-based signing and verification:**
A signature algorithm similar to ECDSA is implemented on an elliptic curve.

- **Key generation:**
When the application is opened, a random private key and a corresponding public key are generated.

- **Message hashing:**
The message's hash value is calculated using the SHA-256 algorithm.

- **Signature generation:**
The user enters a message, and a signature consisting of the `(r, s)` pair is generated for the message.

- **Signature Verification:**

The user can verify by entering the hash and signature information.

- **User Interface:**
- **Signature Creation Page**: Enter a message and create a signature.

- **Signature Verification Page**: Verification is performed using the hash and signature information.

---

## 🛠 Requirements

- Python
- Tkinter (bundled with Python)
- hashlib (standard library for SHA-256 hashing)

No additional installation required.
