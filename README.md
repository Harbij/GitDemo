# Password Toolkit — ICS344 HW
Small toolkit for password checking, user storage (PBKDF2), blacklist Bloom filters, dictionary cracking simulation and AES-GCM file encryption.
## Table of Contents
* [Author](#author)
* [Description](#description)
* [Environment](#environment)
* [CLI Commands](#cli-commands)
* [Bloom filter parameters and expected false-positive rate](#bloom-filter-parameters-and-expected-false-positive-rate)
* [Notes and Limitations](#notes-and-limitations)
* [Troubleshoot](#troubleshoot)
* [Additional Information](#additional-information)

## Author
- Jude Alharbi: [@Harbij](https://www.github.com/Harbij)
## Description
This project implements:
- A simple password-strength meter (entropy estimate + checks).
- PBKDF2-HMAC-SHA256 user creation and verification.
- Bloom-filter blacklist builder and membership tester.
- Dictionary-based crack simulator (recomputes PBKDF2 per candidate).
- AES-GCM file encryption/decryption using user-derived keys.
  
## Environment
- Python: PyCharm
- Platform: Windows
- Files used: password_toolkit.py, data/blacklist.txt, data/dictionary.txt, data/bloom.bin, data/sample_users.json
  
## CLI Commands
| Command | Description |
|----------|--------------|
| `python password_toolkit.py check-password --password P` | Evaluate password strength |
| `python password_toolkit.py create-user --username U --password P` | Create a user with salted PBKDF2 hash |
| `python password_toolkit.py build-bloom --blacklist data/blacklist.txt --out data/bloom.bin` | Build Bloom filter from blacklist |
| `python password_toolkit.py check-bloom --password P` | Check if password appears in Bloom filter |
| `python password_toolkit.py encrypt-file --username U --password P --infile IN --outfile OUT --aad "ICS344"` | Encrypt file with AES-GCM |
| `python password_toolkit.py decrypt-file --username U --password P --infile IN --outfile OUT --aad "ICS344"` | Decrypt AES-GCM encrypted file |
| `python password_toolkit.py simulate-crack --dict data/dictionary.txt` | Attempt to crack stored users using dictionary file |

## Bloom filter parameters and expected false-positive rate
- n: number of blacklist entries
- m: number of bits in the filter
- k: number of hash functions
- p: target false-positive rate
- p_est: expected false-positive rate computed from m, k, n
- Formulas: m = - (n * ln p) / (ln 2)^2 and k = (m / n) * ln 2

## Notes and Limitations
- Bloom filter may reject safe passwords due to false positives.
- AAD must remain identical during encryption and decryption.
- AES-GCM encryption must never reuse nonces with the same key

## Troubleshoot
- If create-user says password is blocked by Bloom, either regenerate Bloom or use a different test password.
- If simulate-crack shows 0 cracks, ensure the dictionary file contains the exact plaintext used when creating test users
  
## Additional Information
- The python file and report are uploaded.
