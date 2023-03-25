
---
title: "Documentation"
linkTitle: "Documentation v0.10"
weight: 20
---

Stevedore is a tool for building Docker images at scale, and it is not intended to replace Dockerfile or Buildkit. Instead, Stevedore can be used in conjunction with these tools to help streamline the process of building and promoting multiple Docker images.

One of the key benefits of using Stevedore is that it provides a consistent and modular way to build Docker images, which can be helpful when dealing with large and complex projects that require multiple images. By using a set of "workers" to perform specific tasks during the build process, you can create a more efficient and automated process for building and promoting Docker images.

Overall, Stevedore is a useful tool for anyone who needs to build and manage large numbers of Docker images, and it can help to improve the overall experience of building and promoting Docker images at scale.

## Why stevedore?

Stevedore is a helpful tool when you need to **build bunches of Docker images** and build them in a **standardized way**. It lets you define how to build your Docker images as well as the relationship between those images. Having that relationship, you can **build automatically the children's images** when the parent image is ready.

To build Docker images, you can use **multiple sources as build context**, for example, building images using a folder from your local host or a git repository. And not only that, but you can also merge several sources to create the ultimate build context.

You can **generate tags automatically** when the main tag is **semantic version 2.0.0**, following the version tree.

Finally, Stevedore provides a **credentials store**. You can store there your credentials and Stevedore uses them to authenticate on your behalf to a Docker registry when you want to push or **promote Docker images**. It also provides the capability to authenticate to AWS ECR.

## Features

### Standardize
Build your Docker images in a standardized way. Create a Dockerfile and reuse it for as many use cases as you need

### Relatives build
Define Docker images relationship and build the relatives images automatically

### Credentials
Store credentials to log in to your Docker registry, AWS Elastic Container Registry or git a server

### Promote
Promote or copy images among Docker registries. Promote from and to AWS Elastic Container Registry is also supported

### Semver awareness
Generate tags automatically when the main tag is semantic version 2.0.0

### Multiple build contexts
Build your images using multiple build context sources. Use local files, a git repository or merge several sources