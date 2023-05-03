
---
title: "Documentation"
linkTitle: "Documentation"
weight: 20
menu:
  main:
    weight: 20
aliases:
  - /documentation
---

Stevedore is a tool for building Docker images at scale, and it is not intended to replace Dockerfile or Buildkit. Instead, Stevedore can be used in conjunction with these tools to help streamline the process of building and promoting multiple Docker images.

One of the key benefits of using Stevedore is that it provides a consistent way to build Docker images and its descendant images, which can be helpful when dealing with large and complex projects that require multiple images. You can also create a more efficient and automated process for building and promoting Docker images.

Overall, Stevedore is a useful tool for anyone who needs to build and manage large numbers of Docker images, and it can help to improve the experience of building and promoting Docker images at scale.

## Why stevedore?
Stevedore simplifies the building of Docker images in a standardized way, with the ability to define relationships between them.

You can build images from multiple sources, including local files and git repositories, and generate automatic tags for semantic versioning.

Stevedore also offers a credentials store for easy authentication to Docker registries and AWS ECR, making image promotion and pushing seamless.

## Features

### Standardize
Build your Docker images in a standardized way. Create a Dockerfile and reuse it for as many use cases as needed

### Relatives build
Automate the building of related Docker images by defining their relationships and dependencies

### Credentials
Store credentials to log in to your Docker registry, AWS Elastic Container Registry or a Git server

### Promote
Easily promote or copy Docker images from one registry to another using Stevedore's image promotion feature

### Semver awareness
Automatically generate tags for semantic version 2.0.0 when building Docker image

### Multiple build contexts
Build your images using multiple build context sources. Use local files, a git repository or merge several sources
