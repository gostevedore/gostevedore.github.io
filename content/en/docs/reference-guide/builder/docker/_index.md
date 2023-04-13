---
title: Docker driver options
tags: ["reference", "builder"]
weight: 3
description: >
  Docker driver options specification
---

The following table describes the attributes that can be configured on the Docker driver.

## Keywords reference for Docker driver
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**context**|map/list(map)|The Docker build context is a set of files required to build a Docker image. It can be located on a local `path` or in a `git` repository.<br>Starting from Stevedore v0.11, you can define a list of contexts that get merged to create the ultimate Docker build context.<br><font color="#AA0088">_mandatory_</font>|Each context type has its own specifications.<br> Refer to the [Docker build contexts]({{<ref "/docs/reference-guide/builder/docker/#docker-build-contexts">}}) section for a detailed description of how to configure each context.|
|**dockerfile**|*string*|`Dockerfiles`'s location path. The path is relative to context root.<br><font color="#AA0088">*optional*</font>|By default, is used the `Dockerfile` located at the root of the context.|

### Docker build contexts
Stevedore supports two kinds of Docker build context. The [path context]({{<ref "/docs/reference-guide/builder/docker/#path-context">}}), which uses a local folder to create the Docker build context, and the [git context]({{<ref "/docs/reference-guide/builder/docker/#git-context">}}), which creates the Docker build context from a git repository.

#### Path context

|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**path**|string|It specifies where to find the files required to build the image.<br><font color="#AA0088">_mandatory_</font>|-|

[Here]({{<ref "/docs/reference-guide/builder/docker/#path-context-example">}}) you have an example where you can see all the options of the path context.

##### Path context example
The goal of the following example is to show you all the configuration options for the path context.

{{<highlight yaml "linenos=table">}}
builder:
  driver: docker
  options:
    dockerfile: Dockerfile.alpine
    context:
      - path: /src/my-app
{{</highlight >}}

#### Git context
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**git**|map|It is the keyword under which are defined the git repository details.<br>Refer [here]({{<ref "/docs/reference-guide/builder/docker/#git-attributes">}}) to know the accepted attributes.<br><font color="#AA0088">_mandatory_</font>|-|

[Here]({{<ref "/docs/reference-guide/builder/docker/#git-context-example">}}) you have an example where you can see all the options of the git context.

##### Git attributes
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**auth**| map | You can provide authorization mechanisms over a Git server to achive the Docker build context from there.<br>Refer [here]({{<ref "/docs/reference-guide/builder/docker/#git-authorization">}}) to know more about the available auth mechanisms.<br><font color="#AA0088">*optional*</font> | - |
|**path**| string | Is the relative path within the repository, to use as build context. | The root of the repository is considered the default path. |
|**repository**| string | Repository specifies the git repository to find the files to build the image. `repository` keyword is placed under `git`.<br><font color="#AA0088">_mandatory_</font>|-|
|**reference**| string | Repository reference required to build the image. `reference` keyword is placed under `git`.<br><font color="#AA0088">*optional*</font>|By default, is used [`refs/heads/master`](https://pkg.go.dev/github.com/go-git/go-git/v5@v5.0.0/plumbing#ReferenceName) reference, that belongs to [go-git](https://github.com/go-git/go-git) project.|

##### Git authorization
Stevedore enables you to obtain authorization from a Git server using basic authentication, which involves providing a username and password, or by using a private key. Alternatively, the SSH agent on your system can be used as a fallback method.

You can provide the necessary information for obtaining authorization either by directly setting the `auth` attribute or by specifying a credential ID, in which case the information will be retrieved from the credential store.

|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**username**| string | This attribute provides the username for the basic authentication method. | - |
|**password**| string | This attribute provides the password for the basic authentication method. | - |
|**git_ssh_user**| string | The username to use when authenticating. | git |
|**private_key_file**| string | The private_key_file attribute is used to provide the path to the private key file. | - |
|**private_key_password**| string | Holds the password for the private key file. | - |
|**credentials_id**| string | Is the ID of the credential used to retrieve the authorization information from the credentials store. | - |

##### Git context example
The goal of that example is to show you all the configuration options for the git context, including the various authentication mechanisms that can be used to access a Git repository.

{{<highlight yaml "linenos=table">}}
builder:
  driver: docker
  options:
    dockerfile: Dockerfile.alpine
    context:
      - git:
          repository: git@gitserver.stevedore.test:/git/repos/my-app.git
          reference: v0.1.2
          path: src
          auth:
            username: user
            password: pAss
            git_ssh_user: git
            private_key_file: /home/user/.ssh/id_rsa
            private_key_password: s3cr3t
            credentials_id: gitserver.stevedore.test
{{</highlight >}}

## Variables-mapping reference

Docker driver passes `variables mapping` to Docker API as build arguments and each variable can be consumed as an [ARG](https://docs.docker.com/engine/reference/builder/#arg) inside the Dockerfile definition.

|Key name|Description|Default<br>argument-name|Default<br>argument-value|
|---|---|---|---|
|**image_from_name_key**|This is the argument-name that you can use to set the name of the base image that you want to use as a starting point for your Docker image. | image_from_name | The argument-value is set as the parent image’s name within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}). |
|**image_from_registry_host_key**|This is the argument-name that you can use to set the Docker registry host where the base image is located. | image_from_registry_host |The argument-value is set as the parent image’s registry host within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}). |
|**image_from_registry_namespace_key**|This is the argument-name that you can use to set the namespace of the Docker registry where the base image is located. | image_from_registry_namespace | The argument-value is set as the parent image’s namespace within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}). |
|**image_from_tag_key**|This is the argument-name that you can use to set the tag of the base image that you want to use as a starting point for your Docker image. | image_from_tag | The argument-value is set as the parent image’s version within the [images-tree]({{<ref "/docs/getting-started/concepts/#images-tree">}}). |


## Docker driver example
The following example is provided for illustrative purposes only and should not be copied and pasted into your configuration.

The goal of the following example is to show you all the configuration options for the Docker driver.

{{<highlight yaml "linenos=table">}}
builder:
  driver: docker
  options:
    dockerfile: Dockerfile.alpine
    context:
      - path: /src/my-app
      - git:
          repository: git@gitserver.stevedore.test:/git/repos/build.git
          reference: v0.1.2
          path: dockerfiles
          auth:
            username: user
            password: pAss
            git_ssh_user: git
            private_key_file: /home/user/.ssh/id_rsa
            private_key_password: s3cr3t
            credentials_id: gitserver.stevedore.test
    variables_mapping:
      image_from_name_key: image_from_name_custom
      image_from_registry_host_key: image_from_registry_host_custom
      image_from_registry_namespace_key: image_from_registry_namespace_custom
      image_from_tag_key: image_from_tag_custom
{{</highlight >}}

And finally here you have the example of the Dockerfile
{{<highlight dockerfile "linenos=table">}}
ARG image_from_registry_host_custom
ARG image_from_registry_namespace_custom
ARG image_from_name_custom
ARG image_from_tag_custom
FROM ${image_from_registry_host_custom}/${image_from_registry_namespace_custom}/${image_from_name_custom}:${image_from_tag_custom}
RUN <set-your-action>
{{</highlight >}}
