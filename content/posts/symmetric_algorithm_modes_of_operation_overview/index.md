---
title: 'Symmetric Algorithm Modes of Operation Overview'
date: 2024-09-11T21:01:05-07:00
draft: false
hidden: false
externalURL: false
showDate: true
showModDate: true
showReadingTime: true
showTags: true
showPagination: true
invertPagination: true
showToC: true
openToC: false
showComments: false
showHeadingAnchors: true
tags: ['Encryption']
---
## Encryption Overview
Encryption can be a rough subject to unpack and understand. I'll do my best here to break things down in digestable chunks.

### Symmetric vs Asymmetric vs Cipher

Symmetric encryption algorithms are a method that uses one key to both encrypt and decrypt plaintext data.

Asymmetric encryption algorithms are a method that uses two keys: a public key and a private key.

Cipher is the algorithm used for performing the encryption or decryption. 

### Encryption Modes

**Block** -- plaintext is broken into *blocks* and the encryption occurs on each block as a single unit.
**Stream** -- each individual byte of the plaintext is encrypted.

Encryption algorithms are often considered a *block cipher* or a *stream cipher*

### Why Modes of Operation Matter

Different modes here than the Encryption Modes detailed above.

*Modes of Operation* are needed when the plaintext block sizes are larger than what the encryption algorithm, aka cipher, would require. For example, if that block size is small, e.g. 128-bit blocks, compared to the plaintext data we want to encrypt (longer than 128-bits) then the algorithm requires a *mode of operation*. These modes will define how each block of the plaintext data is transformed into our ciphertext. These modes also determine how the previous ciphertext affects future encryption operations, as we will see below.

{{< callout >}}
So ***Modes of Operation*** work on ***Block Ciphers*** which fall into the ***symmetric encryption algorithms***
{{< /callout >}}

## Electronic Codebook (ECB)

This is the simplest mode, with each plaintext block being encrypted separately and independently from each other.

- Each block of plaintext is encrypted with the same symmetric key
- There is no feedback mechanism, so each block of plaintext produces the same block of ciphertext

| Advantages |  | Disadvantages |
|---|---|---|
|Simple & Fast to implement, process can be parallelizable |  | Insecure for large plaintext data |
| Ideal for small, independent blocks of plaintext data, such as encryption keys | | Identical plaintext blocks produce identical ciphertext blocks; patterns easy to spot in data |
| | | Susceptible to padding oracle attacks and cryptanalysis | 

{{< callout >}}
Use of ECB is typically discouraged for most practical encryption use cases. With few exceptions, such as encrypting individual keys.
{{< /callout >}}

## Cipher Block Chaining (CBC)

This is considered more secure than ECB. Each block of plaintext is XOR-ed with the previous ciphertext block before it. It does require an initialization vector (IV) to encrypt the first block (again, using XOR with the plaintext).

| Advantages |  | Disadvantages |
|---|---|---|
| Added security by making the ciphertext for each block dependent on the previous one | | Process is sequential which is slower in performance than ECB |
| Patterns in the plaintext are obscured | | Requires proper handling of IV for security |
| Resistive to cryptanalysis | | Vulnerable to padding oracle attacks |

{{< callout >}}
Used in file encryption and secure communication, but typically has been replaced by more secure and efficient methods such as CTR and GCM
{{< /callout >}}

## Counter (CTR)

This mode turns a block cipher into a stream cipher. Instead of using a feedback mechanism, it instead encrypts a counter that changes with each block.

- Each plaintext block is XOR-ed with an encryption counter
- Every iteration increments the counter
- The count can be from an initialization vector (IV)

| Advantages |  | Disadvantages |
|---|---|---|
| Highly parallelizable; fast to encrypt and decrypt | | The IV must never be reused, or security is compromised |
| Patterns are avoided due to counter value changing for each block | | |

{{< callout >}}
Highly efficient at encryption and decryption make it suited for high-speed applications like disk encryption and network communications
{{< /callout >}}

## Output Feedback (OFB)

A stream cipher mode. Each block of ciphertext is used to generate a key stream block which is then used to XOR the plaintext.

- Starts with encrypting the IV and using that ciphertext as input for the next block
- Each subsequent block of ciphertext is then XOR-ed with the plaintext to create the next ciphertext block
- This process repeats for each block of ciphertext

| Advantages |  | Disadvantages |
|---|---|---|
| Errors in one block don't propagate to each subsequent block | | Reusing an IV can compromise security |
| | | Less efficient than CTR mode |

{{< callout >}}
Less common today compared to CTR. Was suitable for stream-oriented communications
{{< /callout >}}

## Galois/Counter Mode (GCM)

This one combines the Counter Mode (CTR) for encryption with the Galois mode of operation for authentication. Too technical for right now, but the arithmetic used is the Galois field to compute the authentication part of this mode.

- Similar to CTR: plaintext is XOR-ed with an encrypted counter
- Provides authenticated encryption, which adds integrity and authenticity
- Authenticated Encryption with Associated Data (AEAD) method

| Advantages |  | Disadvantages |
|---|---|---|
| Provides encryption and authentication in one pass | | Reuse causes catastrophic security issues |
| Highly parallelizable, high performance | | |

{{< callout >}}
Widely used in communication protocols like TLS, IPSec, WPA3
{{< /callout >}}

## Comparison Table

| Mode | Security | Performance | Parallelizable | Main Use Case | Weakness/Concern |
|---|---|---|---|---|---|
|ECB|Weak (patterns visible)|Fast|Yes|Small, independent data blocks|Not secure for large data sets|
|CBC|Stronger (patterns hidden)|Slower|No|File encryption, secure communication|Sequential, padding attacks|
|CTR|Strong|Fast|Yes|Disk encryption, network security|IV/nonce must never be reused|
|OFB|Strong|Fast|Yes|Stream encryption, secure channels|IV reuse issue, less efficient|
|GCM|Strong + Integrity|Fast|Yes|TLS, IPsec, secure communications|IV reuse is catastrophic|