# docker-image

A docker image to facilitate building and publishing docker images to the gitlab
container registry. It supports building images for multiple architectures,
combining them in a single tag with a manifest.

## Usage

Define a job using this image like this:

``` yaml
job:
  image: registry.alpinelinux.org/alpine/infra/docker/exec/docker-image:v2022-09-28
  script: [pwd]
  variables:
    EXEC_COMMAND: build
```

By default it's expected that the Docker file is in the root of the project,
unless `COMPONENT` and/or `SUBDIR` are defined (see below).

## Commands

| Command       | Description                                                               |
|---------------|---------------------------------------------------------------------------|
| build         | Build the image and optionally push it to the registry to a ci repository |
| build_publish | Build and push the image to the registry.                                 |
| manifest      | Create a manifest combining all architectures.                            |

## Variables

The behavior of `docker-image` can be altered by specifying several variables:

| Variable         | Description                                                                                                                                                                                     |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ARCH             | The architecture to build the image for. `build` will publish images with the architecture appended to the tag.                                                                                 |
| COMPONENT        | The name of the component to build. This will affect the directory the Dockerfile is expected to be located in as well as the repository the image is published as.                             |
| SUBDIR           | Override the context directory. Defaults to `COMPONENT` if not defined.                                                                                                                         |
| DOCKER_TAG       | The tag to publish the image as. When using multiple architectures, the final manifest will have this tag, otherwise the individually published image will have this tag. Defaults to `latest`. |
| MANIFEST_ARCHES  | The architectures that are included in the manifest.                                                                                                                                            |
| EXEC_COMMAND     | The command to execute in a CI environment.                                                                                                                                                     |
| PUBLISH_CI_IMAGE | If defined, the `build` command will push the built image to the registry to a separate ci docker repository.                                                                                   |

It expects the following variables to be present:

* CI_REGISTRY
* CI_REGISTRY_USER
* CI_REGISTRY_PASSWORD
* CI_REGISTRY_IMAGE
* CI_COMMIT_SHORT_SHA

Optionally, it will use the following variables

* CI_COMMIT_REF_SLUG
* CI_COMMIT_TAG
* CI_JOB_NAME_SLUG

