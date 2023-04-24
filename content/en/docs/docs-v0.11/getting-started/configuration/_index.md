---
tags: ["getting-started", "configuration"]
title: "Configuration"
linkTitle: "Configuration"
weight: 2
description: >
  Understand the multiple configuration options available in Stevedore
---

## Stevedore configuration
Stevedore can load its configuration either from a local configuration file or environment variables.

### Configuration from environment variables
When you want to use environment variables to define a Stevedore configuration parameter, you must create an environment variable with the same name as the parameter, and prefix it with `STEVEDORE_`. The environment variables must be uppercased.

For example, to define the [images_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#images_path">}}) parameter as an environment variable, you must create the `STEVEDORE_TREE_PATH` variable.

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
{{% pageinfo %}}
Desprecated configuration parameter, it will be removed on version 0.12.0. Please use [builders_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#builders_path">}}) instead.
{{% /pageinfo %}}
It specifies the file that Stevedore uses to look for the definition of the [builders]({{<ref "/docs/docs-v0.11/getting-started/concepts/#builder">}}).
- Environment variable: `STEVEDORE_BUILDER_PATH`
- Default value:
```yaml
   builder_path: stevedore.yaml
```

### **builders_path**
It specifies the location path where Stevedore looks for the definition of the [builders]({{<ref "/docs/docs-v0.11/getting-started/concepts/#builder">}}).

This can be a file or directory. If you configure a directory, Stevedore loads the builders defined in each file within the directory.
- Environment variable: `STEVEDORE_BUILDERS_PATH`
- Default value:
```yaml
   builders_path: stevedore.yaml
```

### **builders**
The [builders]({{<ref "/docs/docs-v0.11/getting-started/concepts/#builder">}}) block contains the global builders definition. 

The location of the builder definitions is specified in [builders_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#builders_path">}}). Stevedore looks for the builder definitions in the file or directory specified by builders_path. For more details, please see the [reference guide]({{<ref "/docs/docs-v0.11/reference-guide/builder/#global-builder">}}).

- Environment variable: `STEVEDORE_BUILDERS`

### **build_on_cascade**
{{% pageinfo %}}
Desprecated configuration parameter, it will be removed on version 0.12.0. To build an image on cascade, please use the `--cascade` flag in the [build]({{<ref "/docs/docs-v0.11/reference-guide/cli/#build">}}) command.
{{% /pageinfo %}}
On build command, indicates whether to start to build children's images once the image build finishes.
- Environment variable: `STEVEDORE_BUILD_ON_CASCADE`
- Default value:
```yaml 
   build_on_cascade: false
```

### **concurrency**
This parameter sets the number of workers that Stevedore creates to build Docker images. Each worker corresponds to one image that can be built concurrently.
- Environment variable: `STEVEDORE_CONCURRENCY`
- Default value: is the rounded integer value of the result of dividing the total number of CPUs by 4. For example, if the system has 16 CPUs, the default value would be 4.

### **credentials**
The credentials configuration block defines the credentials store used to authenticate with external Docker registries or a git server.
You can set the following parameters to for the credentials store:
#### encryption_key
The encryption_key parameter specifies the secret key used to encrypt and decrypt credentials. It is required to provide an encryption key when using `envvars` as the storage type for credentials.

- Environment variable: `STEVEDORE_CREDENTIALS_ENCRYPTION_KEY`
- Default value: null
  
#### format
The format used to store the credentials. Currently supported formats are `JSON` and `YAML`.
- Environment variable: `STEVEDORE_CREDENTIALS_FORMAT`
- Default value: "json"
  
#### local_storage_path
When you use the local credentials store, it defines the local folder where to store the credentials on disk.
- Environment variable: `STEVEDORE_CREDENTIALS_LOCAL_STORAGE_PATH`
- Default value: "credentials"

#### storage_type
The storage_type parameter defines the type of storage backend to use for persisting the credentials. Currently supported types are `local` and `envvars`.

- Environment variable: `STEVEDORE_CREDENTIALS_STORAGE_TYPE`
- Default value: "local"

### **docker_registry_credentials_dir**
{{% pageinfo %}}
Desprecated configuration parameter, it will be removed on version 0.12.0. Refer to the [credentials]({{<ref "/docs/docs-v0.11/getting-started/configuration/#credentials">}}) configuration block to know more about how to define the credentials parameters.
{{% /pageinfo %}}
This option specifies the directory where Docker registry credentials are stored for persistence.
- Environment variable: `STEVEDORE_DOCKER_REGISTRY_CREDENTIALS_DIR`
- Default value:
```yaml 
    docker_registry_credentials_dir: ~/.config/stevedore/credentials
```

### **images_path**
It is the configuration parameter that specifies the location where Stevedore searches for [images definition]({{<ref "/docs/docs-v0.11/getting-started/concepts/#image">}}). 

It can be set to either a file or directory. When the `images_path` configuration parameter is set to a directory, Stevedore loads the definitions from all files within that directory

- Environment variable: `STEVEDORE_IMAGES_PATH`
- Default value:
```yaml
   images_path: stevedore.yaml
```

### **images_tree**
{{% pageinfo %}}
Desprecated configuration parameter, it will be removed on version 0.12.0. Please use [images]({{<ref "/docs/docs-v0.11/getting-started/configuration/#images">}}) instead..
{{% /pageinfo %}}
The `images_tree` block contains the [images-tree]({{<ref "/docs/docs-v0.11/getting-started/concepts/#images-tree">}}), where are placed the [images definition]({{<ref "/docs/docs-v0.11/reference-guide/image/">}}).
  
Stevedore looks for the images on the file set at [images_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#images_path">}}) and loads the images defined under the `images_tree` key. For further information see the [reference guide]({{<ref "/docs/docs-v0.11/reference-guide/images-tree/">}}).

- Environment variable: `STEVEDORE_IMAGES_TREE`
  
### **images**

The `images` block in the Stevedore configuration contains the image definitions. 

Stevedore looks for these definitions in the file or directory set at [images_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#images_path">}}), and loads any images defined under the `images` key. For more information, please refer to the Stevedore [reference guide]({{<ref "/docs/docs-v0.11/reference-guide/images-tree/">}}).

- Environment variable: `STEVEDORE_IMAGES`

### **log_path**
Is the configuration parameter that specifies the location of the log file.

- Environment variable: `STEVEDORE_LOG_PATH`
- Default value:
```yaml 
    log_path: /dev/null
```

### **num_workers**
{{% pageinfo %}}
Desprecated configuration parameter, it will be removed on version 0.12.0. Please use [concurrency]({{<ref "/docs/docs-v0.11/getting-started/configuration/#concurrency">}}) instead.
{{% /pageinfo %}}

It defines the amount of workers to create to build images. That corresponds to the number of images that can be build concurrently.
- Environment variable: `STEVEDORE_NUM_WORKERS`
- Default value: is the rounded integer value of the result of dividing the total number of CPUs by 4. For example, if the system has 16 CPUs, the default value would be 4.

### **push_images**
The `push_images` configuration parameter specifies whether Stevedore should automatically push the built images to the registry after the build process is complete.

- Environment variable: `STEVEDORE_PUSH_IMAGES`
- Default value:
```yaml 
    push_images: true
```

### **semantic_version_tags_enabled**
When set to true, Stevedore generates additional tags based on the semantic version tree tags, if the main image tag is compliant with semantic version 2.0.0.
- Environment variable: `STEVEDORE_SEMANTIC_VERSION_TAGS_ENABLED`
- Default value:
```yaml
    semantic_version_tags_enabled: false
```

### **semantic_version_tags_templates**
 It is a list of templates that are used to generate additional tags when the [semantic_version_tags_enabled]({{<ref "/docs/docs-v0.11/getting-started/configuration/#semantic_version_tags_enabled">}}) configuration parameter is enabled and the main image tag is semver 2.0.0 compliant.
- Environment variable: `STEVEDORE_SEMANTIC_VERSION_TAGS_TEMPLATES`
- Default value:
```yaml 
    semantic_version_tags_templates:  
    - "{{ .Major }}.{{ .Minor }}.{{ .Patch }}"
```

### **tree_path**
{{% pageinfo %}}
Desprecated configuration parameter, it will be removed on version 0.12.0. Please use [images_path]({{<ref "/docs/docs-v0.11/getting-started/configuration/#images_path">}}) instead.
{{% /pageinfo %}}

Is the images tree location path.
- Environment variable: `STEVEDORE_TREE_PATH`
- Default value:
```yaml
    tree_path: stevedore.yaml
```
## Configuration example
Here you have an example with all the parameters available
```yaml
builders_path: stevedore.yaml
builders:
  apps:
    driver: docker
    options:
      context:
        - path: /apps
credentials:
  storage_type: local
  local_storage_path: /etc/stevedore/credentials
  format: json
  encryption_key: thatisadummyencryptionkey
docker_registry_credentials_dir: ~/.config/stevedore/credentials
images_path: stevedore.yaml
images:
  my-app:
    "2.1.0":
      name: "{{ .Name }}"
      version: "{{ .Version }}"
      registry: registry.stevedore.test
      builder: apps
log_path: /var/log/stevedore.log
concurrency: 4
push_images: true
semantic_version_tags_enabled: true
semantic_version_tags_templates:
  - "{{ .Major }}.{{ .Minor }}.{{ .Patch }}"
```