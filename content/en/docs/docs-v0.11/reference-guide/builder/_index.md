---
title: Builder
tags: ["reference", "builder"]
weight: 2
description: >
  Determine how to build Docker images by specifying the relevant parameters
---

A builder provides relevant information about how to build a Docker image. It is defined using a [YAML](https://en.wikipedia.org/wiki/YAML) data structure, and its schema is described on [Keywords reference]({{<ref "/docs/docs-v0.11/reference-guide/builder/#keywords-reference-for-builder-configuration">}}) for builders configuration.

There are two types of builders: _global_ and _in-line_. Global builders are defined in a builders configuration file and can be used by any image. Information on how to set the location path for the builders configuration file can be found in the [configuration]({{<ref "/docs/docs-v0.11/getting-started/configuration/#builders_path">}}) documentation.

In-line builders, on the other hand, are defined within the image definition itself.

## Global builder
A global builder must be defined under the `builders` block inside the Stevedore configuration. It means that Stevedore looks for the `builders` block within the file defined in [builders_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#builders_path">}}) configuration parameter.

You can also set a directory on the [builders_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#builders_path">}}). There you can create several files with the `builders` block defined on them. In that case, Stevedore loads the builders found within all files.

{{< highlight Yaml "linenos=table" >}}
builders:
  builder1:
    driver: docker
    options:
      context:
        path: ./my-apps-golang
  builder2:
    driver: docker
    options:
      context:
        path: ./my-apps-phyton
  builder3:
    driver: ansible-playbook
    options:
      playbook: my-apps-base/site.yml
      inventory: my-apps-base/all
{{</highlight >}}
The previous example defines three builders: `builder1`, `builder2` and `builder3`, all of them are defined under the `builders` block.

Through Stevedore [CLI]({{<ref "/docs/docs-v0.11/reference-guide/cli/">}}) command, you can retrieve the value of the [builders_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#builders_path">}}) configuration parameter.
```bash
$ stevedore get configuration

 builders_path: stevedore.yaml
 concurrency: 4
 semantic_version_tags_enabled: false
 images_path: stevedore.yaml
 push_images: false
 semantic_version_tags_templates:
   - {{ .Major }}.{{ .Minor }}.{{ .Patch }}
 credentials:
   storage_type: local
   format: json
   local_storage_path: credentials
   encryption_key: 12345asdfg

```

## In-line builder
When you want to create an image using an ad-hoc builder, you can provide it in the [image definition]({{<ref "docs/reference-guide/image/">}}) itself. In that case, the builder is known as an in-line builder.

In-line builders are defined following the [Keywords reference]({{<ref "/docs/docs-v0.11/reference-guide/builder/#keywords-reference">}}) for builders' configuration as well.

{{< highlight Yaml "linenos=table" >}}
my-image-base:
  "0.0.1":
    registry: my-registry.my-domain.cat 
    namespace: library
    builder:
      driver: docker
      options:
        context:
          path: my-image-base
{{</highlight >}}

## Keywords reference

The coming section describes the keyword attributes required to define a builder.

|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**name**|*string*|Name of the builder<br><font color="#AA0088">*optional*</font>|When you create a global builder, by default its name is inherited from the key used within the builders' data structure.<br> If the builder is defined as an in-line builder, its name is set as the image's name.|
|**driver**|*string*|Driver to be used to build the image<br><font color="#AA0088">*mandatory*</font>|The allowed values are:<br> - **ansible-playbook**<br> - **docker**|
|**options**|map|Options hold those parameters required by the driver<br><font color="#AA0088">*mandatory*</font>|Each driver requires its own configuration parameters.<br>Refer to the [options reference]({{<ref "/docs/docs-v0.11/reference-guide/builder/#builder-options">}}) for a detailed description.|
|**variables_mapping**|map|Stevedore creates automatically a bunch of parameters, such as the image name or image version.<br> The variables mapping is a key-value data structure where you can override the name of those variables that Stevedore creates and passes to drivers.<br><font color="#AA0088">*optional*</font>|Each driver requires its own variables mapping.<br>Refer to the [variables mapping]({{<ref "/docs/docs-v0.11/reference-guide/builder/#variables-mapping-reference">}}) for a detailed description.|

### Builder options
Builder options are defined in a YAML data structure, and each driver has its own unique set of options. To learn more about which options are accepted by a particular driver, refer to the corresponding section:
- [**Keywords reference for Ansible playbook driver options**]({{<ref "/docs/docs-v0.11/reference-guide/builder/ansible-playbook/#keywords-reference-for-ansible-playbook-driver">}})
- [**Keywords reference for Docker driver options**]({{<ref "/docs/docs-v0.11/reference-guide/builder/docker/#keywords-reference-for-docker-driver">}})

### Variables-mapping reference
Stevedore always sends a set of parameters to the driver when building a Docker image. These parameters are organized in a key-value data structure called `variables_mapping`. Importantly, these parameters are sent to the driver by Stevedore regardless of whether they have been explicitly specified in the [image]({{<ref "/docs/docs-v0.11/reference-guide/image/">}}) definition or as a [CLI]({{<ref "/docs/docs-v0.11/reference-guide/cli/">}}) flag.

When Stevedore sets a parameter to the driver that comes from the `variables_mapping`, it searches for the corresponding _argument-name_ in the `variables_mapping` dictionary in the builder's definition. Users can override the default name of the argument by providing a new name for that argument in the same `variables_mapping` block.

For example, Stevedore sends the parent name and version to the driver even if the user doesn't request them. These parameters are then passed as build arguments to Docker, making them available in the Dockerfile.

It's important to understand the concepts within the variables-mapping context, such as the _key name_, _argument name_, and _argument value_:

- **key name**: This is used internally by Stevedore to identify an argument to create during the image's building process.
- **argument name**: This is the name of the argument that the driver receives to create a Docker image.  Users can override it in the [builder]({{<ref "/docs/docs-v0.11/getting-started/concepts/#builder">}}) definition.
- **argument value**: This is the value of the argument that the driver receives to create a Docker image. It can be specified in the [image]({{<ref "/docs/docs-v0.11/reference-guide/image/">}}) definition, [CLI]({{<ref "/docs/docs-v0.11/reference-guide/cli/">}}) flags or by Stevedore itself. For more information on how Stevedore sets the argument value using `variables-mapping`, please refer to [variables-mapping for Ansible playbook driver]({{<ref "/docs/docs-v0.11/reference-guide/builder/ansible-playbook/#variables-mapping-reference">}}) or [variables-mapping for Docker driver]({{<ref "/docs/docs-v0.11/reference-guide/builder/docker/#variables-mapping-reference">}}).

Each driver receives a distinct set of parameters coming from variables-mapping. Refer to the following links to know more about the variables that each driver receives:
- [**Variables-mapping for Ansible playbook driver**]({{<ref "/docs/docs-v0.11/reference-guide/builder/ansible-playbook/#variables-mapping-reference">}})
- [**Variables-mapping for Docker driver**]({{<ref "/docs/docs-v0.11/reference-guide/builder/docker/#variables-mapping-reference">}})

## Examples
### Docker driver using path context
The following example demonstrates how to define a builder that uses Docker as the driver, with a local build context and a Dockerfile located at `build/Dockerfile`:
{{<highlight yaml "linenos=table">}}
code:
  driver: docker
  options:
  context:
    path: /src/my-app
  dockerfile: build/Dockerfile
{{</highlight >}}

### Docker driver using git context
The following example demonstrates how to define a builder that uses Docker as the driver, with a remote Git repository as the build context and a Dockerfile located at `build/Dockerfile`. Besides, the Git repository `https://github.com/apenella/simple-go-helloworld.git` is used as the build context with the reference `v1.2.3`.
{{<highlight yaml "linenos=table">}}
code:
  driver: docker
  options:
    context:
      git: 
        repository: https://github.com/apenella/simple-go-helloworld.git
        reference: v1.2.3
    dockerfile: build/Dockerfile
{{</highlight >}}

### Ansible playbook driver
The following example shows how to define a builder that uses the ansible-playbook driver. In this example, the builder optiosn defines the ansible inventory and playbook, pointing to the Ansible inventory file located at inventory/all and the playbook file located at build_applications.yml.

{{<highlight yaml "linenos=table">}}
infrastructure:
  driver: ansible-playbook
  options:
    inventory: inventory/all
    playbook: build_applications.yml
{{</highlight>}}