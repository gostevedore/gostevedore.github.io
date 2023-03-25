---
tags: ["reference", "driver"]
title: "Driver"
linkTitle: "Driver"
weight: 3
description: > 
  Know more about the tools you can use to build Docker images
---

A driver is a tool that already knows how to build docker images. Stevedore is an intermediate between the Docker end-user and those tools which can perform a Docker image build.

Based on the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}), the [builder]({{<ref "/docs/getting-started/concepts/#builder">}}) options and the [stevedore CLI]({{<ref "/docs/reference-guide/cli">}}) command flags, Stevedore prepares all required parameters that define the Docker image and passes them to the driver to build the Docker image.

To build a Docker image, a driver receives the parameters defined on the builder's [variables-mapping]({{<ref "/docs/getting-started/concepts/#variables-mapping">}}), the image [vars]({{<ref "/docs/reference-guide/image/#keywords-reference-for-image-configuration">}}) and the image [persistent_vars]({{<ref "/docs/reference-guide/image/#keywords-reference-for-image-configuration">}}).
