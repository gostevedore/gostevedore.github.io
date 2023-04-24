---
tags: ["reference", "cli"]
title: "Command line interface"
linkTitle: "CLI"
weight: 9
description: >
  This page shows the Stevedore's available commands 
---

## build
Stevedore command to build images.

```
Usage:
  stevedore build <image> [flags]

Examples:
stevedore build ubuntu-base --image-version impish --tag 21.10 --pull-parent-image --push-after-build --remove-local-images-after-push

Flags:
      --ansible-connection-local                     When this flag is enabled, ansible uses local connection [only applies to ansible-playbook driver]
      --ansible-intermediate-container-name string   Name of an intermediate container that can be used during ansible build process [only applies to ansible-playbook driver]
      --ansible-inventory-path string                Specify inventory hosts' path or comma separated list of hosts [only applies to ansible-playbook driver]
      --ansible-limit string                         Further limit selected hosts to an additional pattern [only applies to ansible-playbook driver]
      --build-on-cascade                             When this flag is enabled, children images are also built
      --builder-name string                          [DEPRECATED FLAG] use 'ansible-intermediate-container-name' instead of 'builder-name'
      --cascade                                      [DEPRECATED FLAG] use 'build-on-cascade' instead of 'cascade'
      --cascade-depth int                            Number children levels to build when build on cascade is executed (default -1)
      --concurrency int                              Number of images builds that can be excuted at the same time
      --connection-local                             [DEPRECATED FLAG] use 'ansible-connection-local' instead of 'connection-local'
      --dry-run                                      When this flag is enabled, the built is executed in dry-run mode
      --enable-semver-tags                           When this flag is enabled, and main version is semver 2.0.0 compliance extra tag are created based on the semantic version tree
  -h, --help                                         help for build
      --image-from string                            [DEPRECATED FLAG] use 'image-from-name' instead of 'image-from'
  -I, --image-from-name string                       Image parent's name (default "-")
  -N, --image-from-namespace string                  Image parent's registry namespace (default "-")
  -R, --image-from-registry string                   Image parent's registry host (default "-")
  -V, --image-from-version string                    Image parent's version (default "-")
  -i, --image-name string                            Image name. Its value overrides the name on the images tree definition (default "-")
  -r, --image-registry-host string                   Image registry host (default "-")
  -n, --image-registry-namespace string              Image namespace (default "-")
  -v, --image-version strings                        List of versions to build
      --inventory string                             [DEPRECATED FLAG] use 'ansible-inventory-path' instead of 'inventory'
  -l, --label strings                                List of labels to assign to the image
      --limit string                                 [DEPRECATED FLAG] use 'ansible-limit' instead of 'limit'
      --namespace string                             [DEPRECATED FLAG] use 'image-registry-namespace' instead of 'namespace'
      --no-push                                      [DEPRECATED FLAG] 'no-push' is the stevedore default behaviour, use --push-after-build to push an image
      --num-workers int                              [DEPRECATED FLAG] use 'concurrency' instead of 'num-workers'
  -L, --persistent-label strings                     List of persistent labels to set during the build process. Persistent labels inherited from parent has precedence over the current ones. The format of each variable must be <key>=<value>
  -p, --persistent-variable strings                  List of persistent variables to set during the build process. Persistent variable inherited from parent has precedence over the current ones. The format of each variable must be <key>=<value>
      --pull-parent-image                            When this flag is enabled, parent image is pulled from docker registry
      --push-after-build                             When this flag is enabled, the image is pushed to docker registry after the build
      --registry string                              [DEPRECATED FLAG] use 'image-registry-host' instead of 'registry'
      --remove-local-images-after-push               When this flag is enabled, images are removed from local after push
  -T, --semver-tags-template strings                 List of templates to generate tags following semantic version expression
      --set strings                                  [DEPRECATED FLAG] use 'variable' instead of 'set'
      --set-persistent strings                       [DEPRECATED FLAG] use 'persistent-variable' instead of 'set-persistent'
  -t, --tag strings                                  List of extra tags to generate
      --use-docker-normalized-name                   Use Docker normalized name references
  -x, --variable strings                             Variables to set during the build process. The format of each variable must be <key>=<value>

Global Flags:
  -c, --config string   Configuration file location path
      --debug           Enable debug mode
```

## completion
Stevedore command to generate shell completions. [Here](https://github.com/spf13/cobra/blob/master/shell_completions.md) you could find more details about the completion.

```
Usage:
  stevedore completion [flags]

Flags:
  -h, --help   help for completion

Global Flags:
  -c, --config string   Configuration file location path
      --debug           Enable debug mode
```

To load stevedore completion run
```sh
$ . <(stevedore completion)
```

To configure your bash shell to load completions for each session add to your bashrc
```sh
# ~/.bashrc or ~/.profile
. <(stevedore completion)
```

## create
Stevedore command to create items.

```
Usage:
  stevedore create [flags]
  stevedore create [command]

Aliases:
  create, generate

Available Commands:
  configuration Stevedore subcommand to create and initizalize the configuration
  credentials   Stevedore subcommand to create a credentials badge to credentials store

Flags:
  -h, --help   help for create

Global Flags:
  -c, --config string   Configuration file location path
      --debug           Enable debug mode

Use "stevedore create [command] --help" for more information about a command.
```

### create configuration
Stevedore subcommand to create and initialize the configuration.

```
Usage:
  stevedore create configuration [flags]

Aliases:
  configuration, config, conf, cfg

Examples:

Example setting all configuration parameters:
  stevedore create configuration --builders-path /builders --concurrency 4 --config /stevedore-config.yaml --credentials-format json --credentials-local-storage-path /credentials --credentials-storage-type local --enable-semver-tags --force --images-path /images --log-path-file /logs --push-images --semver-tags-template "{{ .Major }}" --semver-tags-template "{{ .Major }}_{{ .Minor }}"


Flags:
  -b, --builders-path string                    It defines the path to locate the builders definition. Its default value is 'stevedore.yaml' (default "stevedore.yaml")
  -c, --concurrency int                         It defines the number of concurrent workers created to build images. Its default value is '4' (default 4)
  -C, --config string                           Configuration file location path
      --credentials-encryption-key string       Is the encryption key used on the credentials store
      --credentials-format string               Format used to store credentials. The accepted formats are: json and yaml (default "json")
      --credentials-local-storage-path string   When is used the 'local' storage, it defines the path to store the credentials. Its default value is 'credentials' (default "credentials")
      --credentials-storage-type string         It defines the storage type. Its default value is 'local' (default "local")
  -s, --enable-semver-tags                      Generate extra tags when the main image tags is semver 2.0.0 compliance. Its default value is 'false'
      --force                                   Force to create configuration file when the file already exists
      --generate-credentials-encryption-key     It creates a random encryption key for the credentials store
  -h, --help                                    help for configuration
  -i, --images-path string                      It defines the path to locate the images definition. Its default value is 'stevedore.yaml' (default "stevedore.yaml")
  -l, --log-path-file string                    Log file location path. Its default value is ''
  -p, --push-images                             On build, push images automatically after it finishes. Its default value is 'false'
  -t, --semver-tags-template strings            List of templates which define those extra tags to generate when 'semantic_version_tags_enabled' is enabled. Its default value is '[{{ .Major }}.{{ .Minor }}.{{ .Patch }}]' (default [{{ .Major }}.{{ .Minor }}.{{ .Patch }}])

Global Flags:
      --debug   Enable debug mode

```

### create credentials
Stevedore subcommand to add a credential to the credentials store.

```
Usage:
  stevedore create credentials [flags]

Aliases:
  credentials, auth, badge

Examples:

Create credentials to authenticate through basic auth into a private registry:
  stevedore create credentials myregistry --username username

Create credentials to achieve credentials from AWS ECR using the default credentials chain:
  stevedore create credentials ecr-host --aws-region eu-west-1 --aws-use-default-credentials-chain

Flags:
      --allow-use-ssh-agent                    When this flag is enabled, is allowed to use ssh-agent
      --ask-private-key-password               When this flag is enabled, you will be asked for a private key password
      --aws-access-key-id string               AWS Access Key ID to achieve credentials from AWS to achieve credentials from AWS. AWS Secret asked key is going to be requested
      --aws-profile string                     AWS Profile to achieve credentials from AWS
      --aws-region string                      AWS Region to achieve credentials from AWS
      --aws-role-arn string                    AWS Role ARN to achieve credentials from AWS
      --aws-shared-config-files strings        List of AWS shared config files to achieve credentials from AWS
      --aws-shared-credentials-files strings   List AWS shared credentials files to achieve credentials from AWS
      --aws-use-default-credentials-chain      When is used that flag, AWS default credentials chain is used to achieve credentials from AWS
  -d, --credentials-dir string                 [DEPRECATED FLAG] 'credentials-dir' is deprecated and will be ignored. Credentials parameters are set through the 'credentials' section of the configuration file or using the flag 'local-storage-path'
      --force                                  When this flag is enabled, credentials creation is forced. It overwrites the existing value
      --git-ssh-user string                    Git SSH User
  -h, --help                                   help for credentials
      --local-storage-path string              Path where credentials are stored locally, using local storage type
      --private-key-file string                Private Key File
  -r, --registry-host string                   [DEPRECATED FLAG] credentials id must be passed as command argument instead of using 'registry-host' flag
      --username string                        Username for basic auth method. Password is going to be requested

Global Flags:
  -c, --config string   Configuration file location path
      --debug           Enable debug mode
```

## get
Stevedore command to get items information.

```
Usage:
  stevedore get [flags]
  stevedore get [command]

Aliases:
  get, list

Available Commands:
  builders      Stevedore subcommand to get builders information
  configuration Stevedore subcommand to get configuration information
  credentials   Stevedore subcommand to get credentials information
  images        Stevedore subcommand that show detail about images definition

Flags:
  -h, --help   help for get

Global Flags:
  -c, --config string   Configuration file location path
      --debug           Enable debug mode

Use "stevedore get [command] --help" for more information about a command.
```

### get builders
Stevedore subcommand to get builders information.

```
Usage:
  stevedore get builders [flags]

Aliases:
  builders, builder, b

Examples:

Get builder filtered by name:
  stevedore get images --filter name=golang-app

Get builder filtered by driver:
  stevedore get images --filter driver=docker


Flags:
  -f, --filter strings   List of filters to apply. Filters must be defined on the following format: <attribute>=<value>
  -h, --help             help for builders

Global Flags:
  -c, --config string   Configuration file location path
      --debug           Enable debug mode

```

### get configuration
Stevedore subcommand to get configuration information.

```
Usage:
  stevedore get configuration [flags]

Aliases:
  configuration, config, conf, cfg

Examples:

  stevedore get configuration


Flags:
  -h, --help   help for configuration

Global Flags:
  -c, --config string   Configuration file location path
      --debug           Enable debug mode

```

### get credentials
Stevedore subcommand to get credentials information.

```
Usage:
  stevedore get credentials [flags]

Aliases:
  credentials, auth, auths, cred, creds, credential

Example:
    stevedore get credentials

Flags:
  -h, --help           help for credentials
      --show-secrets   When this flag is enabled, the output provide secrets

Global Flags:
  -c, --config string   Configuration file location path
      --debug           Enable debug mode

```

### get images
Stevedore subcommand that shows detail about the defined images.

```
Usage:
  stevedore get images [flags]

Aliases:
  images, image, i, img

Examples:

Get images filtered by name:
  stevedore get images --filter name=app1

Get images filtered by registry:
  stevedore get images --filter registry=registry.test


Flags:
  -f, --filter strings               List of filters to apply. Filters must be defined on the following format: <attribute>=<value>
  -h, --help                         help for images
  -t, --tree                         When this flag is enabled, output is returned in tree format
      --use-docker-normalized-name   Use Docker normalized name references

Global Flags:
  -c, --config string   Configuration file location path
      --debug           Enable debug mode
```

## init
Stevedore init command is an alias for [`stevedore create configuration`]({{<ref "/docs/docs-v0.11/reference-guide/cli/#create-configuration">}}) command.

## promote
Stevedore command to promote, publish or copy images to a docker registry or namespace.

```
Usage:
  stevedore promote [flags]

Aliases:
  promote, publish, copy

Examples:
stevedore promote ubuntu:impish --romote-image-registry myregistry.example.com --promote-image-namespace mynamespace

Flags:
  -D, --dry-run                                              Dry run promotion
  -S, --enable-semver-tags                                   When this flag is enabled, and main version is semver 2.0.0 compliance extra tag are created based on the semantic version tree
  -s, --force-promote-source-image                           When this flag is enabled, the source image is also promoted, along with any other target image
  -h, --help                                                 help for promote
  -i, --promote-image-name string                            Target image name (default "-")
  -r, --promote-image-registry-host string                   Target image registry host (default "-")
  -n, --promote-image-registry-namespace string              Target image registry mamespace (default "-")
  -t, --promote-image-tag strings                            List of target image tags
      --remove-local-images-after-push                       When this flag is enabled, images are removed from local after push
      --remove-promote-tags remove-local-images-after-push   [DEPRECATED FLAG] use remove-local-images-after-push instead of `remove-promote-tags`
  -T, --semver-tags-template strings                         List templates to generate tags following semantic version expression
      --use-docker-normalized-name                           Use Docker normalized name references
  -R, --use-source-image-from-remote                         When this flag is enabled, source images is downloaded from remote Docker registry

Global Flags:
  -c, --config string   Configuration file location path
      --debug           Enable debug mode
```

## version
Stevedore command to print the binary release version.

```
Usage:
  stevedore version [flags]

Flags:
  -h, --help   help for version

Global Flags:
  -c, --config string   Configuration file location path
      --debug           Enable debug mode
```



