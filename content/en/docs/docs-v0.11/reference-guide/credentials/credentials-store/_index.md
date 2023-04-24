---
tags: ["credentials"]
title: "Credentials store"
linkTitle: "Credentials store"
weight: 5
description: >
  Store and retrieve credentials
---

The credential store in Stevedore allows users to securely persist and retrieve credentials during the Docker image building process.

Credentials would be used either to pull parent images or to push the outcome image after the build. Another use case for the credentials is when you want to get authorized to a Git server to use a repository as Docker build context.

## Storage type
Stevedore offers two types of storage for credentials: [local storage]({{<ref "/docs/docs-v0.11/reference-guide/credentials/credentials-store/#local-storage">}}), which persists the credentials on the user's local disk, and [envvars storage]({{<ref "/docs/docs-v0.11/reference-guide/credentials/credentials-store/#envvars-storage">}}), which stores the credentials as environment variables.

The storage type used by default is `local` storage. And credentials are stored in the `credentials` folder in your current working directory.

### Local storage
To store the credentials locally on your disk, set the [storage_type]({{<ref "/docs/docs-v0.11/getting-started/configuration/#storage_type">}}) attribute in the credential configuration to `local`.

There you can also set the location to store the credentials. To configure that, use the [local_storage_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#local_storage_path">}}) attribute.

Local storage allows the encryption of the credential. Whenever you set the [encryption_key]({{<ref "/docs/docs-v0.11/getting-started/configuration/#encryption_key">}}) in the credentials configuration, local storage will be encrypted. Each credential is encrypted individually and stored in a file within the credentials storage path.

### Envvars storage
Another option for storing [credentials]({{<ref "/docs/docs-v0.11/reference-guide/credentials">}}) in Stevedore is using the envvars storage type. To do so, you can set the [storage_type]({{<ref "/docs/docs-v0.11/getting-started/configuration/#storage_type">}}) attribute in the credential configuration to `envvars`.

When using envvars storage, credentials are stored as environment variables. This can be particularly useful when working with systems that rely on environment variables, such as continuous integration (CI) or deployment pipelines.

For security concerns, envvars storage encrypts each credential, therefore it needs you to set the [encryption_key]({{<ref "/docs/docs-v0.11/getting-started/configuration/#encryption_key">}}) in the credentials configuration.

To create a new credential when using the envvars storage type, you must use the Stevedore CLI [create credentials]({{<ref "/docs/docs-v0.11/reference-guide/cli/#create-credentials">}}) command. However, please note that this command does not automatically create the corresponding environment variable on your system. Instead, the command will output the environment variable name and value that you will need to create manually.

It's also important to note that credentials stored using envvars storage are not persisted between sessions. This means that if you close your terminal session or restart your machine, you will need to set the credentials again.

Stevedore creates the environment variables to store a credential prefixed by  `STEVEDORE_ENVVARS_CREDENTIALS` and the name is followed by the hash of the credentials id.

That is an example of an environment variable that stores a credential.
```sh
STEVEDORE_ENVVARS_CREDENTIALS_82E99D42EE1191BB42FBFB444920104D=4c1cedd441cd45f8aca66d6e9bfc81...
```

## Security
The credentials store uses [AES-GCM (Galois Counter Mode)](https://en.wikipedia.org/wiki/Galois/Counter_Mode) encryption to securely store each credential before storing it. 

AES-GCM is recommended for securing data at rest because it is a strong encryption algorithm that provides both confidentiality and integrity of the data. It is widely used in various security standards such as FIPS 140-2, which is a US government standard for cryptographic modules, and it is also recommended by NIST (National Institute of Standards and Technology) for use in data encryption.

Additionally, to ensure that the encryption key used is robust, Stevedore generates a hash of the provided credential key, which is used as the encryption key. By using this approach, we can guarantee that the encryption key has 32 characters in length.