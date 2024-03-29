---
tags: ["examples"]
title: "Examples"
linkTitle: "Examples"
weight: 3
description: >
  Discover a variety of practical examples that showcase the capabilities and usage of Stevedore
---

Explore a variety of practical examples that demonstrate the capabilities and usage of Stevedore. These examples provide a cumulative learning experience, building upon one another to deepen your knowledge of Stevedore.

Each example focuses on a specific scenario or use case, guiding you through the necessary steps to achieve the desired outcome. By working through these examples, you will gain hands-on experience and valuable insights into building and managing Docker images with Stevedore. They will empower you to leverage Stevedore's powerful features and enhance your projects with confidence.

## List of examples

| Example | Description |
|---|---|
| [01-basic-example](https://github.com/gostevedore/stevedore/tree/main/examples/01-basic-example) | This example aims to introduce you to Stevedore and its basic concepts and commands. It follows the [quickstart guide](https://gostevedore.github.io/docs/getting-started/quickstart/) from the documentation, which serves as a starting point to get familiar with Stevedore. |
| [02-wildcard-version-example](https://github.com/gostevedore/stevedore/tree/main/examples/02-wildcard-version-example) | This example serves as an introduction to the [wildcard version](https://gostevedore.github.io/docs/getting-started/concepts/#wildcard-version) feature in Stevedore. |
| [03-envvars-credentials-store-example](https://github.com/gostevedore/stevedore/tree/main/examples/03-envvars-credentials-store-example)| This example demonstrates how to use environment variables as the [credentials store](https://gostevedore.github.io/docs/reference-guide/credentials/credentials-store/) in Stevedore. |
| [04-define-multiple-apps-example](https://github.com/gostevedore/stevedore/tree/main/examples/04-multiple-apps-example)| This example showcases how to define and manage multiple applications in Stevedore, allowing you to build and push their Docker images. |
| [05-build-on-cascade-example](https://github.com/gostevedore/stevedore/tree/main/examples/05-build-on-cascade-example)| This example demonstrates the concept of building images on a cascade in Stevedore. It showcases how to create a foundational image with a common configuration and then build multiple applications using that image as a base, streamlining the image building process and ensuring consistency across the applications. |
| [06-git-build-context-example](https://github.com/gostevedore/stevedore/tree/main/examples/06-git-build-context-example)| This example  illustrates how to utilize the [Git build context](https://gostevedore.github.io/docs/reference-guide/builder/docker/#git-context) feature in Stevedore. It demonstrates the ability to specify a Git repository as the build context, allowing you to directly build images from a specific branch, tag, or commit. |
| [07-inject-dockerfile-example](https://github.com/gostevedore/stevedore/tree/main/examples/07-inject-dockerfile-example)| This example showcases the capability of creating a unique build context from separate sources. One of those sources is used to inject a Dockerfile used to build the Docker image. |
| [08-create-semver-tags-automatically-example](https://github.com/gostevedore/stevedore/tree/main/examples/08-create-semver-tags-automatically-example)| This example showcases how to use the SemVer specification to create automatically new image tags for a Docker image. |
| [09-custom-variables-mapping-example](https://github.com/gostevedore/stevedore/tree/main/examples/09-custom-variables-mapping-example)| This example demonstrates the usage of custom variables mapping in Stevedore, allowing for flexible configuration and customization of build arguments provided to the Dockerfile. |
| [10-use-ansible-playbook-driver-example](https://github.com/gostevedore/stevedore/tree/main/examples/10-use-ansible-playbook-driver-example)| This example demonstrates the usage of the [Ansible playbook driver](https://gostevedore.github.io/docs/reference-guide/driver/ansible-playbook/) in Stevedore, enabling you to build and publish Docker images using Ansible playbooks. |
| [11-promote-images-example](https://github.com/gostevedore/stevedore/tree/main/examples/11-promote-images-example)| This example highlights the image promotion feature in Stevedore, demonstrating how to promote Docker images from one Docker registry to another. |
| [12-copy-images-from-dockerhub-example](https://github.com/gostevedore/stevedore/tree/main/examples/12-copy-images-from-dockerhub-example)| This example illustrates how to copy Docker images from Docker Hub to your local Docker registry using Stevedore, allowing you to have a local copy of the desired images for offline or restricted environments. |
