---
group: cloud-guide
title: ece-tools package
functional_areas:
  - Cloud
---

The `{{site.data.var.ct}}` package is a set of scripts and tools designed to manage and deploy {{site.data.var.ece}} projects. The `{{site.data.var.ct}}` package simplifies many {{site.data.var.ece}} processes, such as Docker environment deployment, cron management, and project verification. You can view and contribute to the open-source [ece-tools repository on Github](https://github.com/magento/ece-tools).

{% include cloud/note-ece-tools-package.md %}

The `{{site.data.var.ct}}` package is compatible with {{site.data.var.ee}}—starting with version 2.1.4—and contains scripts and {{site.data.var.ece}} commands designed to help manage your code and automatically build and deploy your projects.

The following lists the available `{{site.data.var.ct}}` commands:

```bash
php ./vendor/bin/ece-tools list
```

## Build and deploy

The `{{site.data.var.ct}}` package contains commands to perform operations for the build, deploy, and post-deploy stages of launching your {{site.data.var.ece}} application. For example, the `php ./vendor/bin/ece-tools build` command begins the application build process.

By default, these `{{site.data.var.ct}}` commands are in the [hooks property][hooks] of the `.magento.app.yaml` configuration file.

## Docker configuration generator

The `{{site.data.var.ct}}` package includes a dependency for the {{site.data.var.mcd}} package, which provides functionality and Docker images to [launch a Docker development environment]({{ site.baseurl }}/cloud/docker/docker-config.html) for Magento Cloud. You can also run Magento Cloud Docker as a stand-alone package.

You use the following commands to generate the Docker configuration files and build your environment.

Command | Action
:------ | :------
`ece-docker build:compose` | Builds the docker environment in [production mode][mode] by default and verifies configured service versions.
`ece-docker build:compose --mode="developer"` | Builds the docker environment in [developer mode][mode].
`ece-docker build:compose --mode="production"` | Builds the docker environment in [production mode][mode].
`ece-docker image:generate:php` | Convert PHP configuration files to Docker ENV files.

The following example lists the `{{site.data.var.mcd}}` Docker commands:

```bash
php ./vendor/bin/ece-docker list
```

Sample response:

```terminal
Available commands:
  help                Displays help for a command
  list                Lists commands
 build
  build:compose       Build docker configuration
  build:dist          Generates Docker .dist files
 image
  image:generate:php  Generates proper configs
 build
  build:compose       Build docker configuration
  build:dist          Generates Docker .dist files
 image
  image:generate:php  Generates proper configs
```
{:.no-copy}

See [Docker development] to learn more about using Magento Cloud Docker for development and testing your {{site.data.var.ece}} projects.

## Apply patches

The `{{site.data.var.ct}}` package includes a dependency for the {{site.data.var.mcp}} package, which automatically applies Magento patches when you deploy to a Cloud environment. You can also use {{site.data.var.mcp}} to apply custom patches to your project. See [Apply patches]

## Services, routes, and variables

You can use the `{{site.data.var.ct}}` package to display detailed information about the Base64-encoded [Cloud variables][cloudvar] used in any Cloud environment. The following command shows all services, routes, and variables.

```bash
php ./vendor/bin/ece-tools env:config:show
```

To display a specific set of information, use the following format:

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

-  `services`—Displays the relationship data from the `MAGENTO_CLOUD_RELATIONSHIPS` environment variable, defined in the `services.yaml` file.
-  `routes`—Displays the configured routes for the project using the `MAGENTO_CLOUD_ROUTES` environment variable.
-  `variables`—Displays the configured variables for the project using the `MAGENTO_CLOUD_VARIABLES` environment variable.

Sample output for the `services` option:

```terminal
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| elasticsearch:                                                       |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```
{:.no-copy}

## Verify environment configuration

{{site.data.var.ece }} provides a set of verification commands to help evaluate the configuration of your project. See [Smart wizards][wizard] in the _Optimize deployment_ section for a detailed description of each wizard command. The `wizard:ideal-state` command runs automatically during the build phase. To verify the ideal state of your project:

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

 {:.bs-callout-info}
You must run the `wizard:ideal-state` command in the Cloud environment. The command always returns the `The configured state is not ideal` error when run in the local development environment.

Sample output:

```terminal
Ideal state is configured
```
{:.no-copy}

{% include cloud/note-ece-tools-release-info.md %}

<!-- link definitions -->
[mode]: {{site.baseurl}}/cloud/docker/docker-config.html#launch-modes
[hooks]: {{site.baseurl}}/cloud/project/project-conf-files_magento-app.html#hooks
[cloudvar]: {{site.baseurl}}/cloud/env/variables-cloud.html
[wizard]: {{site.baseurl}}/cloud/deploy/smart-wizards.html
[Docker development]:  {{site.baseurl}}/cloud/docker/docker-development.html
[Apply patches]: {{site.baseurl}}/cloud/project/project-patch.html