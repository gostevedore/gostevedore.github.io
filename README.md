# gostevedore.github.io

![Website publish status](https://github.com/gostevedore/gostevedore.github.io/actions/workflows/publish.yml/badge.svg)

This repository holds the Stevedore site's source code. To visit the site click [here](https://gostevedore.github.io/)

The website is built using [Hugo](https://gohugo.io/), as a static website generator, and [Docsy](https://www.docsy.dev/) theme.

## Deployment process

This repository provides a Github action named `Publish` which manages the deployment.
When a new site's release is ready to be deployed on your development environment you must follow the steps below to perform the deployment:

1) Create a pull request to the `main` branch
2) Merge the pull request, and then the `Publish` action is triggered.
3) Once the `Publish` action is completed properly, the site will be available at https://gostevedore.github.io/

> The `Publish` action pushes the code generated by `Hugo` to `gh-pages` repository's branch.
