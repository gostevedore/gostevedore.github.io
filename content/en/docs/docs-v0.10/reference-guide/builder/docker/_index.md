---
title: Docker driver options
tags: ["reference", "builder"]
weight: 3
description: >
  Docker driver options specification
---

The following table describes the attributes that can be configured on the Docker driver.

## Keywords reference for Docker driver
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**context**|map|It is the Docker build context. There you find the set of files required to build a Docker image. The context cam be located either on a local path, known as `path` context, or in a git respository, named `git` context.<br><font color="#AA0088">_mandatory_</font>|Each context type has its own specifications.<br> Refer to the [Docker build contexts]({{<ref "/docs/docs-v0.10/reference-guide/builder/#docker-build-contexts">}}) section for a detailed description of how to configure each context|
|**dockerfile**|*string*|`Dockerfiles`'s location path. The path is relative to context root<br><font color="#AA0088">*optional*</font>|By default, is used the `Dockerfile` located at context root|

### Docker build contexts
Stevedore supports two kinds of Docker build context. The **path** context, which uses a local folder to create the Docker build context, and the **git** context, which creates the Docker build context from a git repository.

#### Path context
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**path**|string|It specifies where to find the files required to build the image<br><font color="#AA0088">_mandatory_</font>|-|

#### Git context
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**git**|map|It is the keyword under which are defined the git repository details<br><font color="#AA0088">_mandatory_</font>|-|
|**repository**|string|Repository specifies the git repository to find the files to build the image. `repository` keyword is placed under `git`.<br><font color="#AA0088">_mandatory_</font>|-|
|**reference**|string|Repository reference required to build the image. `reference` keyword is placed under `git`.<br><font color="#AA0088">*optional*</font>|By default, is used [`refs/heads/master`](https://pkg.go.dev/github.com/go-git/go-git/v5@v5.0.0/plumbing#ReferenceName) reference, that belongs to [go-git](https://github.com/go-git/go-git) project.|

## Variables-mapping reference

Docker driver passes `variables mapping` to Docker API as build arguments and each variable can be consumed as an [ARG](https://docs.docker.com/engine/reference/builder/#arg) inside the Dockerfile definition.

|Key name|Description|Default<br>argument-name|Default<br>argument-value|
|---|---|---|---|
|**image_from_name_key**|This is the argument-name that you can use to set the name of the base image that you want to use as a starting point for your Docker image|image_from_name|The argument-value is set as the parent image’s name within the [images-tree]({{<ref "/docs/docs-v0.10/getting-started/concepts/#images-tree">}})|
|**image_from_registry_host_key**|This is the argument-name that you can use to set the Docker registry host where the base image is located|image_from_registry_host|The argument-value is set as the parent image’s registry host within the [images-tree]({{<ref "/docs/docs-v0.10/getting-started/concepts/#images-tree">}})|
|**image_from_registry_namespace_key**|This is the argument-name that you can use to set the namespace of the Docker registry where the base image is located|image_from_registry_namespace|The argument-value is set as the parent image’s namespace within the [images-tree]({{<ref "/docs/docs-v0.10/getting-started/concepts/#images-tree">}})|
|**image_from_tag_key**|This is the argument-name that you can use to set the tag of the base image that you want to use as a starting point for your Docker image|image_from_tag|The argument-value is set as the parent image’s version within the [images-tree]({{<ref "/docs/docs-v0.10/getting-started/concepts/#images-tree">}})|


The example below defines a Dockerfile that can use the arguments passed by the docker driver to build a Docker image.
{{<highlight dockerfile "linenos=table">}}
ARG image_from_registry_host
ARG image_from_registry_namespace
ARG image_from_name
ARG image_from_tag
FROM ${image_from_registry_host}/${image_from_registry_namespace}/${image_from_name}:${image_from_tag}
RUN <set-your-action>
{{</highlight >}}
