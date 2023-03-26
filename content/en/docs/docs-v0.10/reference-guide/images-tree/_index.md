---
tags: ["reference", "images-tree"]
title: "Images tree"
linkTitle: "Images tree"
weight: 4
description: >
  Add your images definition into Stevedore's images-tree and create a relationship among them
---

The images-tree is a map of maps data structure that contains the definitions of the images, as well as the relationships between them. It follows the following rules:

1. You define the images-tree in [YAML](https://en.wikipedia.org/wiki/YAML) structure. Stevedore looks for the images-tree in the file specified on the [tree_path]({{< ref "/docs/docs-v0.10/getting-started/configuration/#tree_path" >}}) configuration parameter.
2. The main key containing the tree definition is named `images_tree`. The value is a map of maps data structure.
3. You place all the image names that belong to the images-tree under "images_tree" block. Each image's name is a first-level key.
4. You place the second-level keys under each image name key to identify the image versions.
5. You define the [image]({{<ref "/docs/docs-v0.10/reference-guide/image/">}}) version's value as the image definition for that specific version.
6. Images can be related to each other, and these relationships are defined using the parent name and version in the image definition.
7. Images in the images-tree can be built using Stevedore’s build command, which will build the image and any of its descendants.

{{% pageinfo color="secondary" %}}
In case an image name or image version contains a `.` (_dot_), you must quote it to avoid the YAML parser to fails.
{{% /pageinfo %}}


## Example
The following example specifies an images-tree that provides multiple image definitions for the _ubuntu_, _php-fpm_, and _php-cli_ images. The _ubuntu_ image has two versions defined, _18.04_ and _20.04_, each with its image definition. The value of these versions is the image definition for each one of them. 

Please note that if an image name or version contains a `.` (_dot_), it should be enclosed in quotes to prevent the YAML parser from failing.

{{<highlight Yaml "linenos=table">}}
images_tree:
  ubuntu:
    "18.04":
      builder: global-infr-builder
      children:
      - php-fpm:
        - "8.0"
    "20.04":
      builder: global-infr-builder
      children:
      - php-fpm:
        - "8.1"
      - php-cli:
        - "8.1"
  php-fpm:
    "8.0":
      builder:
        driver: docker
        context:
          path: php-fpm
        vars:
          version: "{{ .Version }}"
    "8.1":
      builder:
        driver: docker
        context:
          path: php-fpm
        vars:
          version: "{{ .Version }}"
  php-cli:
    "8.1":
      builder:
        driver: docker
        context:
          path: php-fpm
        vars:
          version: "{{ .Version }}"
{{</highlight>}}
