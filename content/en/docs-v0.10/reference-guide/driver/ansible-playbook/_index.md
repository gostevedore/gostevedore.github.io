---
title: "Ansible playbook"
linkTitle: "Ansible playbook"
weight: 4
description: >
  This page describes the ansible-playbook driver
---

To create a Docker image using the ansible-playbook driver you need an Ansible playbook to carry out that task.
Since Stevedore executes the ansible-playbook command, you must install the Ansible tool set before building an image.

The driver prepares the parameters on the builder's [variables-mapping]({{<ref "/docs/getting-started/concepts/#variables-mapping">}}), the image [vars]({{<ref "/docs/reference-guide/image/#keywords-reference-for-image-configuration">}}) and the image [persistent_vars]({{<ref "/docs/reference-guide/image/#keywords-reference-for-image-configuration">}}) and passed them as Ansible _extra-vars_. It then utilizes the [go-ansible](https://github.com/apenella/go-ansible) library to execute the corresponding ansible-playbook commands.

## Driver configuration
The configuration of the driver is done through the builder. There you can set the path of the _playbook_ and _inventory_ as well as the variables-mapping.

Refer to the ansible-playbook [reference]({{<ref "/docs/reference-guide/builder/ansible-playbook/#keywords-reference-for-ansible-playbook-driver">}}) to know more about how to set the options for the ansible-playbook driver.

## Recipe to write the Ansible playbook
To write an Ansible playbook that builds a Docker image, you will need to use the [docker collection](https://galaxy.ansible.com/community/docker). This collection provides a set of modules that allow you to interact with the Docker API.

Your playbook should include the following steps:

The Ansible playbook you want to use to create a Docker image must include the following action defined there:
1. Start a Docker container that will be used as the base for the image you want to create. You can use the docker_container module to start the container, specifying the image you want to use as the base.
2. Provision and configure the base container to include any required packages, libraries, or other dependencies. You can use the standard Ansible modules to perform these tasks.
3. Once the container is fully provisioned and configured, you can use the docker_commit module to create a new Docker image based on the modified container.

By following these steps, you can create a Docker image using an Ansible playbook.