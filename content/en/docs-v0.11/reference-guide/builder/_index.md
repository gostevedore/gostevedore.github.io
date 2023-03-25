---
title: Builder
tags: ["reference", "builder"]
weight: 2
description: >
  Determine how to build Docker images by specifying the relevant parameters
---

A builder provides relevant information about how to build a Docker image. It is defined using a [YAML](https://en.wikipedia.org/wiki/YAML) data structure, and its schema is described on [Keywords reference]({{<ref "/docs/reference-guide/builder/#keywords-reference-for-builder-configuration">}}) for builders configuration.

There are two types of builders: global and in-line. Global builders are defined in the builders configuration file and can be used by any image. Information on how to set the location path for the builders configuration file can be found in the [configuration]({{<ref "/docs/getting-started/configuration/#builder_path">}}) documentation.

In-line builders, on the other hand, are defined within the image definition itself.

## Global builder
A global builder must be defined under the `builders` block inside the Stevedore configuration. It means that Stevedore looks for the `builders` block within the file defined in [builder_path]({{<ref "/docs/getting-started/configuration/#builder_path">}}) configuration parameter.

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
The previous example defines three builders: `builder1`, `builder2` and `builder3`, all of them are defined within the `builders` block.

Through Stevedore CLI command, you can retrieve the value of the [builder_path]({{<ref "/docs/getting-started/configuration/#builder_path">}}) configuration parameter.
```bash
$ stevedore get configuration
PARAMETER                           VALUE
tree_path                           stevedore.yaml
builder_path                        stevedore.yaml
log_path                            /dev/null
num_workers                         4
push_images                         true
build_on_cascade                    false
docker_registry_credentials_dir     ~/.config/stevedore/credentials
semantic_version_tags_enabled       false
semantic_version_tags_templates     [{{ .Major }}.{{ .Minor }}.{{ .Patch }}]
```

## In-line builder
When you want to create an image using an ad-hoc builder, you can provide it in the [image definition]({{<ref "docs/reference-guide/image/">}}) itself. In that case, the builder is known as an in-line builder.

In-line builders are defined following the [Keywords reference]({{<ref "/docs/reference-guide/builder/#keywords-reference-for-builder-configuration">}}) for builders configuration as well.

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
|**options**|map|Options hold those parameters required by the driver<br><font color="#AA0088">*mandatory*</font>|Each driver requires its own configuration parameters.<br>Refer to the [options reference]({{<ref "/docs/reference-guide/builder/#builder-options">}}) for a detailed description.|
|**variables_mapping**|map|Stevedore creates automatically a bunch of parameters, such as the image name or image version.<br> The variables mapping is a key-value data structure where you can override the name of those variables that Stevedore creates and passes to drivers.<br><font color="#AA0088">*optional*</font>|Each driver requires its own variables mapping.<br>Refer to the [variables mapping]({{<ref "/docs/reference-guide/builder/#variables-mapping-reference">}}) for a detailed description.|

### Builder options
Builder options are defined in a YAML data structure, and each driver has its own unique set of options. To learn more about which options are accepted by a particular driver, refer to the corresponding section:
- [**Keywords reference for Ansible playbook driver options**]({{<ref "/docs/reference-guide/builder/ansible-playbook/#keywords-reference-for-ansible-playbook-driver">}})
- [**Keywords reference for Docker driver options**]({{<ref "/docs/reference-guide/builder/docker/#keywords-reference-for-docker-driver">}})

### Variables-mapping reference
Stevedore always sends a set of parameters to the driver when building a Docker image. These parameters are organized in a key-value data structure called `variables_mapping`. Importantly, these parameters are sent to the driver by Stevedore regardless of whether they have been explicitly specified in the [image]({{<ref "/docs/reference-guide/image/">}}) definition or as a CLI flag.

When Stevedore sets a parameter, it looks for the corresponding argument-name in the `variables_mapping` block within the builder's definition. Users can override the default value of the argument-name by setting a new value for the argument in the `variables_mapping` block.

It's important to understand the concepts within the variables-mapping context, such as the _key name_, _argument name_, and _argument value_:

- **key name**: This is used internally by Stevedore to identify the name of the argument to use during the image building process.
- **argument name**: This is the name of the argument that the driver receives to create a Docker image.  Users can override it in the [builder]({{<ref "/docs/getting-started/concepts/#builder">}}) definition
- **argument value**: This is the value of the argument that the driver receives to create a Docker image. It can be specified in the [image]({{<ref "/docs/reference-guide/image/">}}) definition or CLI flags.

Each drivers receives a distinct set of parameters comming from variables-mapping. Refer to the following links to know more about variables that each driver receives:
- [**Variables-mapping for Ansible playbook driver**]({{<ref "/docs/reference-guide/builder/ansible-playbook/#variables-mapping-reference">}})
- [**Variables-mapping for Docker driver**]({{<ref "/docs/reference-guide/builder/docker/#variables-mapping-reference">}})

## Examples
### Docker driver using path context
The following example demonstrates how to define a builder that uses Docker as the driver, with a local build context and a Dockerfile located at `build/Dockerfile`:
{{<highlight yaml "linenos=table">}}
code:
  driver: docker
  options:
  context:
    path: .
  dockerfile: build/Dockerfile
{{</highlight >}}

### Docker driver using git context
The following example demonstrates how to define a builder that uses Docker as the driver, with a remote Git repository as the build context and a Dockerfile located at `build/Dockerfile`. In this example, the Git repository `https://github.com/apenella/simple-go-helloworld.git` is used as the build context with the reference `v0.0.0`.
{{<highlight yaml "linenos=table">}}
code:
  driver: docker
  options:
    context:
      git: 
        repository: https://github.com/apenella/simple-go-helloworld.git
        reference: v0.0.0
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