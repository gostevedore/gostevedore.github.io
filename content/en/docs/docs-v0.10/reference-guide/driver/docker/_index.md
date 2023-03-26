---
title: "Docker"
linkTitle: "Docker"
weight: 4
description: >
  This page describes the Docker driver
---

The Docker driver is a Stevedore tool that enables building Docker images using the [Docker Engine API]((https://pkg.go.dev/github.com/docker/docker/client?utm_source=godoc#section-documentation)). To accomplish this, Stevedore uses the [go-docker-builder](https://github.com/apenella/go-docker-builder) library, which provides a set of abstractions on top of the Docker API, allowing Stevedore to prepare the Docker build context and set the build arguments required to build the Docker image. This data is then sent to the Docker Engine API via the [go-docker-builder](https://github.com/apenella/go-docker-builder) library, which creates the Docker image based on the provided parameters.

## Driver configuration
The configuration of the driver is done through the builder. There you can set the path of the Docker's build context and the Dockerfile.

Refer to the Docker [reference]({{<ref "/docs/docs-v0.10/reference-guide/builder/docker/#keywords-reference-for-docker-driver">}}) to know more about how to set the options for the Docker driver.