---
title: "Image"
linkTitle: "Image"
tags: ["reference", "image"]
weight: 1
description: >
  Define the Docker images you want to build
---

Stevedore represents a Docker image by an image definition, therefore an _image_ holds the details required to build a Docker image.
Note that an _image_ by itself has no sense, it must be defined within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}).

## Keywords reference

|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**builder**| string/map | Identifies the [builder]({{<ref "/docs/getting-started/concepts/#builder" >}}) to use to create the image.<br>You can define it as a _string_ when you want to refer to a [global-builder]({{<ref "/docs/reference-guide/builder/#global-builder">}}).<br>To designate it such an [in-line builder]({{<ref "/docs/reference-guide/builder/#in-line-builder">}}), you have to define it as a _yaml data-structure_. In the [builder]({{<ref "/docs/reference-guide/builder/#in-line-builder">}})'s reference guide you have the details to define it.<br>If the builder attribute is not defined, it is used a builder which does not perform actions.<br><font color="#AA0088">*optional*</font> | - |
|**children**| map | Contains references to descendant images which uses the current image as parent.<br><font color="#AA0088">*optional*</font> | It is a map where each key refers to an image name within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}) and the value is a list of image versions.<br><br>Example:<br><pre>children:<br>  child1:<br>  - v1<br>  - v2</pre> |
|**name**| string | It is the image name.<br><font color="#AA0088">*optional*</font> | By default its value is defined as the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}})'s image name key |
|**namespace**| string | It is the Docker`s registry namespace.<br><font color="#AA0088">*optional*</font> | - |
|**parents**| map | Contains the references to the parent images from which the current image can be built.<br><font color="#AA0088">*optional*</font> | It is a map that provides a reference to the parent images within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}). Each key identifies the parent name and the value is a list of versions.<br><br>Example:<br><pre>parents:<br>  parent1:<br>  - v1<br>  - v2</pre> |
|**persistent_vars**| list(map) | You define there a list of variables required to build the Docker image. That variables are inherit by the children images.<br><font color="#AA0088">*optional*</font> | Its value is a key-value data-structure where each key is an _string_ and its value a _yaml data-structure_.<br><br>Example:<br><pre>persistent_vars:<br>  var1: value<br>  var2: value</pre> | - |
|**registry**| string | It is the Docker's registry host.<br><font color="#AA0088">*optional*</font> | - |
|**tags**| list(string) | You define a list of additional tags to generate.<br><font color="#AA0088">*optional*</font> | - |
|**vars**| list(map) | You define there a list of variables required to build the Docker image.<br><font color="#AA0088">*optional*</font> | Its value is a key-value structure where each key is an _string_ and its value a _yaml data-structure_.<br><br>Example:<br><pre>vars:<br>  var1: value<br>  var2: value</pre> | - |
|**version**| string | It is the Docker's image tag.<br><font color="#AA0088">*optional*</font> | By default its value is defined as the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}})'s image version key |
|**persistent_labels**| list(map) | It is a key-value metadata pair that provides descriptive information about the image. That variables are inherit by the children images.<br><font color="#AA0088">*optional*</font> | Its value is a key-value structure where each key is an _string_ and its value a _yaml data-structure_.<br><br>Example:<br><pre>persistent_labels:<br>  label1: value<br>  label2: value</pre> |
|**labels**| list(map) | It is a key-value metadata pair that provides descriptive information about the image.<br><font color="#AA0088">*optional*</font> | Its value is a key-value structure where each key is an _string_ and its value a _yaml data-structure_.<br><br>Example:<br><pre>labels:<br>  label1: value<br>  label2: value</pre> |

## Templating image attributes

An image definition allows you to define the values of its attributes as a [Golang text template](https://pkg.go.dev/text/template). To achieve that, Stevedore provides you with the following template variables.

|Name|Variable|Description|
|---|---|---|
| **name** | `Name` | It provides the image's name within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}) |
| **version** | `Version` | It provides the image's version within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}) |
| **parent** | `Parent` | It let you to achieve the templating image attributes from the parent image. You can access, example to _{{ .Parent.Name }}_ attribute. |
| **date RFC3339** | ` DateRFC3339` | It stores a date and time value in a string format that conforms to the RFC 3339 standard. The format includes the year, month, day, hour, minute, and second components, and optionally the fraction of a second and the time zone offset. |
|**date RFC3339 Nano**| `DateRFC3339Nano` | It refers to a date/time value formatted according to the RFC3339Nano specification, which includes nanosecond precision. This specification defines a standard format for representing dates and times in machine-readable formats, using the ISO 8601 standard. |

## Image definition example
The following example is provided for illustrative purposes only and should not be copied and pasted into your configuration.

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
persistent_labels:
  created_at: "{{ .DateRFC3339Nano }}"
  created_by: stevedore
labels:
  parent_image: "{{ .Parent.Name }}:{{ .Parent.Version }}"
persistent_vars:
  base_version: "{{ .Version }}"
vars:
  os_base: rootfs.tar
parents:
  alpine:
    - "1.36"
  ubuntu:
    - "22.04"
{{</highlight>}}
