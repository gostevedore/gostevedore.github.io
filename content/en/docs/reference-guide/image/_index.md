---
title: "Image"
linkTitle: "Image"
tags: ["reference", "image"]
weight: 1
description: >
  Define the Docker images you want to build
---

Stevedore represents a Docker image by an image definition, therefore an image holds the details required to build that image.
Note that an _image_ by itself has no sense, it must be defined within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}).

## Keywords reference

|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**builder**| string/map | Identifies the [builder]({{<ref "/docs/getting-started/concepts/#builder" >}}) to use to create the image.<br>You can define it as a _string_ when you want to refer to a [global-builder]({{<ref "/docs/reference-guide/builder/#global-builder">}}) already defined.<br>To designate it such an [in-line builder]({{<ref "/docs/reference-guide/builder/#in-line-builder">}}), you must define it as a _yaml data-structure_. In the [builder]({{<ref "/docs/reference-guide/builder/#in-line-builder">}})'s reference guide you find the details to define it.<br>If the builder attribute is not defined, it is used a builder which does not perform actions. | - |
|**children**| map | It is a map where each key refers to an image within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}) and the value is a list of image versions. | - |
|**name**| string | It is the image name. | By default its value is defined as the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}})'s image name key |
|**namespace**| string | It is the Docker`s registry namespace. | - |
|**parents**| map | It is a map that provides a reference to its parent images within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}). Each key identifies the parent image name and the value is a list of parent image versions. | - |
|**persistent_vars**| map | You define there a list of variables required to build the Docker image. That variables are inherit by the children images. | Its value is a key-value data-structure where each key is an _string_ and its value a _yaml data-structure_. | - |
|**registry**| string | It is the Docker's registry host. | - |
|**tags**| list | You define a list of additional tags to generate. | - |
|**vars**| map | You define there a list of variables required to build the Docker image. | Its value is a key-value structure where each key is an _string_ and its value a _yaml data-structure_. | - |
|**version**| string | It is the Docker's image tag. | By default its value is defined as the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}})'s image version key |

### Example
{{<highlight Yaml "linenos=table">}}
name: base-apps
version: 3.2.1
builder:
  driver: docker
  options:
  	context:
	  path: ./base-app
namespace: stable
registry: registry.stevedore.test
tags:
  - latest
persistent_vars:
  base_version: {{ .Version }}
vars:
  os_base: rootfs.tar
parents:
  alpine:
    - "1.36"
  ubuntu:
    - "22.04"
{{</highlight>}}

## Templating image attributes

An image definition allows you to define the values of its attributes as a [Golang text template](https://pkg.go.dev/text/template).

To achieve that, Stevedore provides you with the next template variables, which refer to values from the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}).

|Name|Template|Description|
|---|---|---|
| **name** | _{{ .Name }}_ | It provides the image's name within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}) |
| **version** | _{{ .Version }}_ | It provides the image's version within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}) |
| **parent** | _{{ .Parent }}_ | It let you to achieve the templating image attributes from the parent image. |
| **date RFC3339** | _{{ . DateRFC3339 }}_ | It stores a date and time value in a string format that conforms to the RFC 3339 standard. The format includes the year, month, day, hour, minute, and second components, and optionally the fraction of a second and the time zone offset. |
|**date RFC3339 Nano**| _{{ .DateRFC3339Nano }}_ | It refers to a date/time value formatted according to the RFC3339Nano specification, which includes nanosecond precision. This specification defines a standard format for representing dates and times in machine-readable formats, using the ISO 8601 standard. |

{{<highlight Yaml "linenos=table">}}
name: ubuntu
version: "{{ .Version }}"
builder:
  driver: docker
  options:
  	context:
	  path: ./ubuntu
namespace: stable
registry: registry.stevedore.test
tags:
  - "{{ .Version }}-{{ .Parent.Version }}"
persistent_vars:
  ubuntu_version: "{{ .Version }}"
persistent_labels:
  created_at: "{{ .DateRFC3339Nano }}"
{{</highlight>}}
