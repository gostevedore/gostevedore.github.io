
---
title: "Documentation v0.10"
linkTitle: "Documentation v0.10"
weight: 20
toc_hide: true
description: >
    Explore the documentation for Stevedore v0.10
---

Stevedore is a tool for building Docker images at scale, and it is not intended to replace Dockerfile or Buildkit. Instead, Stevedore can be used in conjunction with these tools to help streamline the process of building and promoting multiple Docker images.

One of the key benefits of using Stevedore is that it provides a consistent and modular way to build Docker images, which can be helpful when dealing with large and complex projects that require multiple images. By using a set of "workers" to perform specific tasks during the build process, you can create a more efficient and automated process for building and promoting Docker images.

Overall, Stevedore is a useful tool for anyone who needs to build and manage large numbers of Docker images, and it can help to improve the overall experience of building and promoting Docker images at scale.

## Why stevedore?
Stevedore simplifies the building of Docker images in a standardized way, with the ability to define relationships between them. You can build images from multiple sources, including local files and git repositories, and generate automatic tags for semantic versioning. Stevedore also offers a credentials store for easy authentication to Docker registries and AWS ECR, making image promotion and pushing seamless.

## Features

### Standardize
Build your Docker images in a standardized way. Create a Dockerfile and reuse it for as many use cases as you need

### Relatives build
Define Docker images relationship and build the relatives images automatically

### Credentials
Store credentials to log in to your Docker registry, AWS Elastic Container Registry or a Git server

### Promote
Promote or copy images among Docker registries. Promote from and to AWS Elastic Container Registry is also supported

### Semver awareness
Generate tags automatically when the main tag is semantic version 2.0.0

### Multiple build contexts
Build your images using multiple build context sources. Use local files, a git repository or merge several sources