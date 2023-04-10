---
tags: ["credentials"]
title: "Credentials"
linkTitle: "Credentials"
weight: 5
description: >
  Load the credentials you require to the store to build Docker images
---

Credentials allow users to securely store authentication information, such as access keys, tokens, passwords, and SSH keys. These credentials can be used to authenticate with Docker registries and Git servers, allowing users to push and pull images and code securely and with ease.

Stevedore uses the [credentials store]({{<ref "/docs/reference-guide/credentials/credentials-store/">}}) to persist and retrieve credentials during the Docker building process.

## Credentials keywords reference
A credential is a data structure with a defined schema.
Although it is possible to create a credential manually, it is recommended to use the [Stevedore CLI]({{<ref "/docs/reference-guide/cli/#create-credentials">}}) to ensure that the credential follows the correct schema.

The following table describes the credentials attributes.
| Keyword | Type | Description | Default value |
|---|:---:|---|---|
| **aws_access_key_id** | string | Is the AWS Access Key ID, a unique identifier for the AWS account that is used to authenticate and authorize API requests made to AWS services. | - |
| **aws_region** | string |  Specifies the AWS region associated with the credentials. The region is used to route requests to the appropriate endpoint for the resource. | - |
| **aws_role_arn** | bool | Specifies the Amazon Resource Name (ARN) of the IAM role to assume when using the credentials. This attribute is expected to be used when the _aws_use_default_credentials_chain_ attribute is set to _true_. | false |
| **aws_secret_access_key** | string | This is a sensitive credential attribute that represents the Secret Access Key associated with an AWS account. | - |
| **aws_profile** | string | This is the name of the AWS profile to use when authenticating with AWS. | - |
| **aws_shared_credentials_files** | list(string) | Specifies the path to the shared credentials file to use. This attribute is expected to be used when the _aws_use_default_credentials_chain_ attribute is set to _true_. | - |
| **aws_shared_config_files** | list(string) | Is a list of shared config files to use. It is used when _aws_use_default_credentials_chain_ attribute is set to _true_. | - |
| **aws_use_default_credentials_chain** | bool | When set to _true_, enables the use of the default credentials chain provided by the AWS SDK. This chain specifies a series of possible locations and methods for the SDK to find and use credentials, such as environment variables, shared credential files, or IAM roles. By setting this attribute to _true_, you can leverage the same default behaviour used by the SDK to automatically discover and load credentials, without having to manually specify each one. For more information on the default credentials chain, you can refer to the official AWS SDK documentation at https://aws.github.io/aws-sdk-go-v2/docs/configuring-sdk/#specifying-credentials. | false |
| **docker_login_password** | string | **DEPRECATED**: It will be removed on version 0.12.0<br>Password password for basic auth method. | - |
| **docker_login_username** | string | **DEPRECATED**: It will be removed on version 0.12.0<br>Username username for basic auth method. | - |
| **id** | string | Is the credential identifier. | - |
| **password** | string | This attribute provides the password for the basic authentication method. It can be used to authenticate with either a Docker registry or a Git server, allowing Stevedore to securely access and retrieve the required resources. | - |
| **username** | string | The username attribute is used for basic authentication and provides the username to authenticate with either a Docker registry or a Git server. | - |
| **private_key_file** | string | The private_key_file attribute is used to provide the path to the private key file used for authentication to a Git server. | - |
| **private_key_password** | string | Holds the password for the private key file, which can be used to authenticate to a Git server. | - |
| **git_ssh_user** | string | The username to use when authenticating via SSH to a Git server. | git |
| **use_ssh_agent** | bool | It must be set to true when you allow using the ssh-agent to authenticate to the Git server. | false |
