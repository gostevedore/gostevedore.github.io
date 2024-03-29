---
title: Ansible playbook driver options
tags: ["reference", "builder"]
weight: 2
description: >
  Ansible playbook driver options specification
---

## Keywords reference for Ansible Playbook driver

The following table describes the attributes that can be configured on the Ansible playbook driver.

|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**playbook**|*string*|It is the playbook file location path<br><font color="#AA0088">*mandatory*</font>|-|
|**inventory**|*string*|It is the  ansible-playbook inventory<br><font color="#AA0088">*mandatory*</font>|It can be either an inventory file or and adhoc inventory, such `127.0.0.1,`|

## Variables-mapping reference

The `ansible-playbook` driver provides to ansible-playbook the variables-mapping entries as [extra-vars](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables).

|Key name|Description|Default<br>argument-name|Default<br>argument-value|
|---|---|---|---|
|**image_builder_label_key**|This is the argument-name to set the name of the intermediate container that is used to create the Docker image using the ansible-playbook driver|image_builder_label|The argument-value is set as the full qualified name of the image|
|**image_fully_qualified_name**|This is the argument-name that you can use to set the fully-qualified image name of of the image that you are building.<br>Introduced in v0.11.2. | image_fully_qualified_name |This is the argument-name that provides you with fully-qualified image name. |
|**image_from_fully_qualified_name**|This is the argument-name that you can use to set the fully-qualified name of the base image that you want to use as a starting point for your Docker image.<br>Introduced in v0.11.2.| image_from_fully_qualified_name |This is the argument-name that provides you with fully-qualified parent image name. |
|**image_from_name_key**|This is the argument-name to set the name of the parent image from which the new image will be built using the ansible-playbook driver|image_from_name|The argument-value is set as the parent image's name within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}})|
|**image_from_registry_host_key**|This is the argument-name to set the parent image's registry host when creating a Docker image using the ansible-playbook driver|image_from_registry_host|The argument-value is set as the parent image's registry host within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}})|
|**image_from_registry_namespace_key**|This is the argument-name to set the parent image's namespace within the registry when creating a Docker image using the ansible-playbook driver|image_from_registry_namespace|The argument-value is set as the parent image's namespace within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}})|
|**image_from_tag_key**|This is the argument-name to set the parent image's tag when creating a Docker image using the ansible-playbook driver.|image_from_tag|The argument-value is set as the parent image's version within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}})|
|**image_name_key**|This is the argument-name to set the name of the new Docker image that will be built using the ansible-playbook driver|image_name|If not specified, the argument-value for the image name will be derived from the name of the image within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}})
|**image_tag_key**|This is the argument-name to set the tag for the new Docker image that will be built using the ansible-playbook driver|image_tag|If not specified, the argument-value for the default tag will be the version of the image within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}})|
|**image_registry_namespace_key**|This is the argument-name that you can use to set the namespace or organization of the registry where the image will be pushed. It allows you to specify the namespace separately from the image name when pushing the image to a registry|image_registry_namespace|There is no default argument-value. If it is not specified, the image will be pushed to the root namespace of the specified registry|
|**image_registry_host_key**|This is the argument-name that you can use to set the Docker registry host when creating a Docker image|image_registry_host|Stevedore leaves the argument-value empty, which means that the Docker Hub registry host is used|
|**image_extra_tags_key**|This is the argument-name that you can use to specify additional tags to be applied to the Docker image when it is built using the ansible-playbook driver. You can specify multiple tags providing a [JSON array](https://json-schema.org/understanding-json-schema/reference/array.html)|image_extra_tags_key|-|
|**push_image_key**|Use this argument-name to specify a variable that controls whether the Docker image should be automatically pushed after it's built|push_image|The argument-value is set as _false_|

## Ansible-playbook driver example

The goal of the following example is to show you all the configuration options for a builder which uses the ansible-playbook driver.

{{<highlight yaml "linenos=table">}}
builder:
  driver: ansible-playbook
  options:
    playbook: /src/my-app/site.yml
    inventory: /src/my-app/inventory.ini
  variables_mapping:
    image_builder_label_key: image_builder_label
    image_from_name_key: image_from_name
    image_from_registry_host_key: image_from_registry_host
    image_from_registry_namespace_key: image_from_registry_namespace
    image_from_tag_key: image_from_tag
    image_name_key: image_name
    image_tag_key: image_tag
    image_registry_namespace_key: image_registry_namespace
    image_registry_host_key: image_registry_host
    image_extra_tags_key: image_extra_tags
    push_image_key: push_image
{{</highlight >}}
