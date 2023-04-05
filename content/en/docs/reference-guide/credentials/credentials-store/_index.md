---
tags: ["credentials"]
title: "Credentials store"
linkTitle: "Credentials store"
weight: 5
description: >
  Store and retrieve credentials
---

The credential store in Stevedore allows users to securely persist and retrieve credentials during the Docker image building process.

Credentials would be used either to pull parent images or to push the outgoing image after the build. Another use case for the credentials is when you want to get authorized to a Git server to use a repository as Docker build context.

## Storage type
Stevedore offers two types of storage for credentials: [local storage]({{<ref "/docs/reference-guide/credentials/credentials-store/#local-storage">}}), which persists the credentials on the user's local disk, and [envvars storage]({{<ref "/docs/reference-guide/credentials/credentials-store/#envvars-storage">}}), which stores the credentials as environment variables.

The storage type used by default is the `local` storage. And credentials are stored in the `credentials` folder in your current working directory.

### Local storage
To store the credentials locally on your disk, set the [storage_type]({{<ref "/docs/getting-started/configuration/#storage_type">}}) attribute in the credential configuration to `local`.

There you can also set the location to store the credentials. To configure that, use the [local_storage_path]({{<ref "/docs/getting-started/configuration/#local_storage_path">}}) attribute.

Local storage allows the credentials encryption. Whenever you set the [encryption_key]({{<ref "/docs/getting-started/configuration/#encryption_key">}}) in the credentials configuration, local storage will be encrypted. Each credential is encrypted individually and stored in a file within the credentials storage path.

### Envvars storage

## Security
The credentials store uses [AES-GCM (Galois Counter Mode)](https://en.wikipedia.org/wiki/Galois/Counter_Mode) encryption to securely store each credential before storing it. 

AES-GCM is recommended for securing data at rest because it is a strong encryption algorithm that provides both confidentiality and integrity of the data. It is widely used in various security standards such as FIPS 140-2, which is a US government standard for cryptographic modules, and it is also recommended by NIST (National Institute of Standards and Technology) for use in data encryption.

Additionally, to ensure that the encryption key used is robust, Stevedore generates a hash of the provided credential key, which is used as the encryption key. By using this approach, we can guarantee that the encryption key has 32 characters length.