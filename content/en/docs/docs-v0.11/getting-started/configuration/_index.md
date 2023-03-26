---
tags: ["getting-started", "configuration"]
title: "Configuration"
linkTitle: "Configuration"
weight: 2
description: >
  This page describes the Stevedore's configuration parameters
---

## Stevedore configuration
Stevedore can load its configuration either from a local configuration file or environment variables.

### Configuration from environment variables
When you want to use environment variables to define a Stevedore configuration parameter, you must create an environment variable with the same name as the parameter, and prefix it with `STEVEDORE_`. The environment variables must be uppercased.

For example, to define the [tree_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#tree_path">}}) parameter as an environment variable, you must create the `STEVEDORE_TREE_PATH` variable.

### Configuration from local configuration file
Stevedore looks for the user configuration on the listed files, and it does following order that appears in the list:
1. ./stevedore.yaml
2. ~/.config/stevedore/stevedore.yaml
3. ~/stevedore.yaml

However, you can load the configuration from a custom location using `--config` flag on stevedore CLI. Loading configuration from a custom location has precedence over any other configuration.

## Create the configuration file
Stevedore CLI provides the command `stevedore create configuration` to create the configuration file. However, you can also make it manually.
The command `stevedore init`, is an alias from the create configuration subcommand that is also available to generate the configuration file.

When you use the CLI to generate the configuration file, the outcome configuration is created at `./stevedore.yaml` but you can indicate your desired location by the `--config` flag.

### Stevedore credentials
Stevedore could store your Docker registry's credentials and use them on your behalf during the [building]({{<ref "/docs/docs-v0.11/getting-started/concepts/#build">}}) or [promoting]({{<ref "/docs/docs-v0.11/getting-started/concepts/#promote">}}) processes.

The default folder to store the credentials is `~/.config/stevedore/credentials`. Nevertheless, you can define where to store the credentials by setting the [docker_registry_credentials_dir]({{<ref "/docs/docs-v0.11/getting-started/configuration/#docker_registry_credentials_dir">}}) parameter.

In case you use the CLI to create Stevedore's configuration, you can also request to create Docker's registry credentials.

Finally, with the CLI command `stevedore create credentials`, you can create as many credentials as you need.

## Configuration parameters
In the coming section are described all the Stevedore configuration parameters.

### **builder_path**
Is the Builders location path.
- Environment variable: `STEVEDORE_BUILDER_PATH`
- Default value:
```yaml
   builder_path: stevedore.yaml
```

### **builders**
It holds the global builders definition. 
- Environment variable: `STEVEDORE_BUILDERS`

Stevedore looks for builders definition on the file set at [builder_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#builder_path">}}) and loads the builders defined under the `builders` key. For further information see the [reference guide]({{<ref "/docs/docs-v0.11/reference-guide/builder/#global-builder">}}).

### **build_on_cascade**
On build command, indicates whether to start to build children's images once the image build finishes.
- Environment variable: `STEVEDORE_BUILD_ON_CASCADE`
- Default value:
```yaml 
   build_on_cascade: false
```

### **docker_registry_credentials_dir**
Is the folder to persist the Docker's registry credentials.
- Environment variable: `STEVEDORE_DOCKER_REGISTRY_CREDENTIALS_DIR`
- Default value:
```yaml 
    docker_registry_credentials_dir: ~/.config/stevedore/credentials
```

### **images_tree**
It holds images definition as well as its relationship. 
- Environment variable: `STEVEDORE_IMAGES_TREE`
  
Stevedore looks for the images on the file set at [tree_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#tree_path">}}) and loads the images defined under the `images_tree` key. For further information see the [reference guide]({{<ref "/docs/docs-v0.11/reference-guide">}}).

### **log_path**
Is the log file location path.
- Environment variable: `STEVEDORE_LOG_PATH`
- Default value:
```yaml 
    log_path: /dev/null
```

### **num_workers**
It defines the amount of workers to create to build images. That corresponds to the number of images that can be build concurrently.
- Environment variable: `STEVEDORE_NUM_WORKERS`
- Default value:
```yaml 
    num_workers: 4
```

### **push_images**
On build command, indicates whether to push images automatically after it finishes.
- Environment variable: `STEVEDORE_PUSH_IMAGES`
- Default value:
```yaml 
    push_images: true
```

### **semantic_version_tags_enabled**
Indicates whether to generate additional tags based on semantic version tree tags, when the main image tag is semver 2.0.0 compliance.
- Environment variable: `STEVEDORE_SEMANTIC_VERSION_TAGS_ENABLED`
- Default value:
```yaml
    semantic_version_tags_enabled: false
```

### **semantic_version_tags_templates**
It provides the list of templates used to create the additional tags when the`semantic_version_tags_enabled` is enabled.
- Environment variable: `STEVEDORE_SEMANTIC_VERSION_TAGS_TEMPLATES`
- Default value:
```yaml 
    semantic_version_tags_templates:  
    - "{{ .Major }}.{{ .Minor }}.{{ .Patch }}"
```

### **tree_path**
Is the images tree location path.
- Environment variable: `STEVEDORE_TREE_PATH`
- Default value:
```yaml
    tree_path: stevedore.yaml
```
## Configuration example
Here you have an example with all the parameters available
```yaml
builder_path: stevedore.yaml
builders:
  apps:
    driver: docker
    options:
      context:
        - path: /apps
build_on_cascade: true
docker_registry_credentials_dir: ~/.config/stevedore/credentials
images_tree:
  my-app:
    "2.1.0":
      name: "{{ .Name }}"
      version: "{{ .Version }}"
      registry: registry.stevedore.demo
      builder: apps
log_path: /var/log/stevedore.log
num_workers: 4
push_images: true
semantic_version_tags_enabled: true
semantic_version_tags_templates:
  - "{{ .Major }}.{{ .Minor }}.{{ .Patch }}"
tree_path: stevedore.yaml
```