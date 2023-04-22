---
tags: ["reference", "images-tree"]
title: "Images tree"
linkTitle: "Images tree"
weight: 4
description: >
  Add your images definition into Stevedore's images-tree and create a relationship among them
---

The _images-tree_ is a data structure that contains the definitions of the images, as well as the relationships between them. It is defined in a [YAML](https://en.wikipedia.org/wiki/YAML) format containing a main key named `images`, which is a map of maps data structure.

Under the `images` block, you place all the image names, and each image’s name is a first-level key. For each image name, you have to define a second level of keys that identifies the image versions. The value of each version is the [image definition]({{<ref "/docs/reference-guide/image/">}}) for that specific name and version.

Note that in case an image name or image version contains a `.` (_dot_), you must quote it to avoid the YAML parser failing. This is a common issue that can be easily solved by wrapping the name or version in quotes.

Images can be related to each other, and these relationships are defined using their name and version. The relationships can be established from the image to its parents or its children. The [images reference guide]({{<ref "/docs/reference-guide/image/">}}) provides more information about how to set the images’ relationship.

To know which images can be built, Stevedore searches for the _images-tree_ in the file specified on the [images_tree]({{< ref "/docs/getting-started/configuration/#images_tree" >}}) configuration parameter, or in a directory specified by the same parameter. In the latter case, Stevedore loads the image definitions found within all the files in the directory. 

## Example
The following example specifies an _images-tree_ that provides multiple image definitions for the _busybox_, _php-fpm_, _php-cli_ and _my-app_ images.

{{<highlight Yaml "linenos=table">}}
images:
  root:
    latest:
      persistent_labels:
        created_at: "{{ .DateRFC3339Nano }}"

  busybox:
    "1.35":
      builder: global-infr-builder
      parents:
        root:
          - latest
    "1.36":
      builder: global-infr-builder
      parents:
        root:
          - latest
  php-fpm:
    "8.0":
      version: "{{ .Version }}-{{ .Parent.Version }}"
      builder:
        driver: docker
        context:
          path: php-fpm
      parents:
        busybox:
          - "1.35"
    "8.1":
      version: "{{ .Version }}-{{ .Parent.Version }}"
      builder:
        driver: docker
        context:
          path: php-fpm
      parents:
        busybox:
          - "1.35"
          - "1.36"
  php-cli:
    "8.1":
      version: "{{ .Version }}-{{ .Parent.Version }}"
      builder:
        driver: docker
        context:
          path: php-cli
      parents:
        busybox:
          - "1.35"
          - "1.36"
  my-app:
    "*":
      version: "{{ .Version }}-{{ .Parent.Version }}"
      builder:
        driver: docker
        context:
          git:
            repository: git@gitserver.stevedore.test:/git/repos/my-app.git
            reference: "{{ .Version }}"
            auth:
              credentials_id: mygit.stevedore.test
      parents:
        php-fpm:
          - "8.1"
{{</highlight>}}

You can check the previously defined images-tree by executing `stevedore get images --tree`.
{{<highlight shell "linenos=table">}}
$ stevedore get images --tree
├─── root:latest
│  ├─── busybox:1.35
│  │  ├─── php-fpm:8.0-1.35
│  │  ├─── php-fpm:8.1-1.35
│  │  │  ├─── my-app:{{ .Version }}-{{ .Parent.Version }}
│  │  ├─── php-cli:8.1-1.35
│  ├─── busybox:1.36
│  │  ├─── php-fpm:8.1-1.36
│  │  ├─── php-cli:8.1-1.36

{{</highlight>}}