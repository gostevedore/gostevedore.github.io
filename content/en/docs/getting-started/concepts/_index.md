---
tags: ["getting-started", "concepts", "builder", "image", "images-tree", "driver"]
title: "Concepts"
linkTitle: "Concepts"
weight: 2
description: >
  Know about the Stevedore concepts and terms that appear along that documentation
---

### Build
The process to generate a Docker image.

### Builder
A Builder arranges a set of parameters to build a Docker image. Among those parameters you can define the [driver]({{<ref "/docs/getting-started/concepts/#driver">}}) to perform the build, the Docker build context or Dockerfile location. 

You can define a builder as a **global builder** or an **in-line builder**. 

Global builders are defined within a folder location specified in the [configuration]({{<ref "/docs/getting-started/configuration/#builders_path">}}) and can be used by any image definition.

On the snipped below there are defined two global builders: `code`, at *line 2* and `global-infr-builder`, at *line 7*.
{{< highlight Yaml "linenos=table" >}}
builders:
  code:
    driver: docker
    options:
      context:
        path: .
  global-infr-builder:
    driver: ansible-playbook
    options:
      inventory: inventory/all
      playbook: build_applications.yml
{{< /highlight >}}

You can define a builder inside the image definition. That builder is known as **in-line builder**.

On the next snipped is defined an in-line builder:
{{< highlight Yaml "linenos=table" >}}
simple-go-helloworld:
  "0.0.1":
    version: "{{ .Version }}"
    builder:
      driver: docker
      options:
        context:
          git: 
            repository: https://github.com/apenella/simple-go-helloworld.git
      variables_mapping:
        image_name_key: image
        image_tag_key: tag
{{< /highlight >}}

### Credentials
Credentials allows users to securely store authentication information, such as access keys, tokens, passwords, and SSH keys. These credentials can be used to authenticate with Docker registries and Git servers, allowing users to push and pull images and code securely and with ease.

### Credentials store
The credential store in Stevedore allows users to securely store and retrieve credentials needed during the Docker image building process.

### Driver
Stevedore relies on existing tools designed for building Docker images rather than implementing its own mechanism. The driver plays a crucial role in this process by preparing the build parameters based on the image and builder definitions and triggering the request to build the image.

As of today, Stevedore supports the following drivers: 
- **docker** driver that uses the Docker API. When a builder is defined to use the docker driver, it must provide at least the context, along with other parameters such as Dockerfiles location. For further information refer to [reference guide]({{<ref "/docs/reference-guide/driver/docker">}}).
- **ansible-playbook** driver builds Docker images by executing ansible playbooks. When a builder is defined to use an ansible-playbook driver, it must provide the playbook location as well as the Ansible inventory. For further information refer to [reference guide]({{<ref "/docs/reference-guide/driver/ansible-playbook">}}).

### Image
An image definition, which is also known as _image_ in Stevedore's context, defines the Docker image you want to build, how to build it and the relationship with its parents or children images.

An image must be defined within the [Images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}), and you can refer to it by its name and version. For further information refer to [reference guide]({{<ref "/docs/reference-guide/image">}}).

{{< highlight Yaml "linenos=table" >}}
my-image-base:
    "0.0.1":
        registry: my-registry.my-domain.cat 
        namespace: library
        tags:
        - latest 
        parents:
          busybox:
            - "1.36"
          ubuntu:
            - "22.04"
        builder:
            driver: docker
            options:
                context:
                    path: my-image-base
{{< /highlight >}}

### Images-tree
The Images-tree is a data-strucutre contains the definition of the images, as well as the relationship among them.

On the snipped below you can see three images defined within the Images-tree: `my-image-base`, at line 2, `my-ms1`, at line 13, and `my-ms2`, at line 35.
{{< highlight Yaml "linenos=table" >}}
images:
    my-image-base:
        "stable":
            registry: my-registry.example.com 
            namespace: library
            tags:
            - latest
            builder:
                driver: docker
                options:
                    context:
                        path: my-image-base
    my-ms1:
        "0.0.1":
            registry: my-registry.example.com 
            namespace: library
            parents:
              my-image-base:
                - stable
        devel:
            registry: my-registry.example.com 
            namespace: library
            version: "{{ .Version }}-{{ .Parent.Name }}{{ .Parent.Version }}"
            parents:
              my-image-base:
                - stable
                - devel
        "*":
            registry: my-registry.example.com 
            namespace: library
            version: "{{ .Version }}"
            parents:
              my-image-base:
                - stable
    my-ms2:
        "0.1.5":
            registry: my-registry.example.com 
            namespace: library
            version: "{{ .Version }}-{{ .Parent.Name }}{{ .Parent.Version }}"
            parents:
              my-image-base:
                - stable
        "*":
            registry: my-registry.example.com 
            namespace: library
            version: "{{ .Version }}"
            parents:
              my-image-base:
                - stable
{{< /highlight >}}

### Promote
Promoting an image, in Stevedore's context, means pushing an image to the Docker registry or another namespace on the same Docker registry.

### Semver tag
When a tag is [semantic version 2.0.0](https://semver.org/) compliance is known as semver tag.

### Wildcard version
The wildcard version provides a fallback definition that can be used to build an image by giving any value to the version.

An image is identified by a name and version, and when you want to build an image using an undefined version, if the image provides a wildcard version definition, that version is used to build the image.

You can recognize when an image has defined a wildcard version definition because the value of the version is  `*` (*start*).

The next snipped provides an image with a wildcard version definition, at line 5. The `{{ .Version }}` could be set as any value used to build the `my-app` image.
{{< highlight Yaml "linenos=table" >}}
my-app:
    "0.1.5":
        registry: my-registry.example.com 
        namespace: library
    "*":
        registry: my-registry.example.com 
        namespace: library
        version: "{{ .Version }}"
{{< /highlight >}}

### Variables mapping
Variables mapping (a.k.a. varmap or varmapping) is a builder's optional attribute that defines the name of those variables that are automatically generated by Stevedore and used by the driver to perform the request to build an image. 

Each driver defines its variables mapping. For further information refer to [variables mapping]({{<ref "/docs/reference-guide/builder/#variables-mapping-reference">}}) reference guide.
