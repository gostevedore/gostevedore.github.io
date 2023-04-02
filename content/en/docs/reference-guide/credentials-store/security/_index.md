---
tags: ["credentials","security"]
title: "Security"
linkTitle: "Security"
weight: 1
description: >
  Know about the security in credentials store
---

The credentials store uses AES-GCM (Galois Counter Mode) encryption to securely store sensitive information such as authentication tokens or passwords. Each credential is encrypted before being stored in the credentials store. 

This encryption algorithm provides strong security measures against unauthorized access or data tampering. AES-GCM is a widely used encryption standard that provides authenticated encryption and integrity protection. This ensures that the credentials remain secure and cannot be accessed by unauthorized parties. 

Additionally, to ensure that the encryption key used is robust, Stevedore generates a hash of the provided credential key, which is used as the encryption key. By using this approach, we can guarantee that the encryption key has a minimum length of 32 characters, making it more difficult to brute-force attacks.

Overall, the use of AES-GCM encryption in the credentials store provides strong security and integrity for stored credentials, helping to ensure that they remain safe from prying eyes.

Benefits of AES-GCM:

- Strong security: AES-GCM provides robust security features, such as authenticated encryption and data integrity checks, which protect against unauthorized access or tampering of sensitive data.

- High-performance: AES-GCM is optimized for modern hardware and software platforms, which means it provides excellent performance even when handling large amounts of data.

- Widely adopted: AES-GCM is a widely accepted encryption algorithm used in many industries, such as finance, healthcare, and government, making it a trustworthy option for securing sensitive data.