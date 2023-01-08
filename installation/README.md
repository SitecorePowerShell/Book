---
description: Review prerequisites and details on how to get setup with SPE.
---

# Installation

## Prerequisites

Take a quick look at the [appendix](../appendix/README.md) to see which version of SPE you should be installing that is compatible with your Sitecore environment. Generally SPE has pretty good backwards compatibility but may require a new version to support the latest Sitecore CMS release.

 
* Windows Management Framework 5.1 (PowerShell) is generally available for most Windows environments. 
* PowerShell [Execution Policy](https://technet.microsoft.com/en-us/library/ee176961.aspx) set to `RemoteSigned` \(probably optional\)

{% hint style="info" %}
The release of SPE 6.0 introduced name changes to some files which are now reflected throughout the documentation. Review [issue #1109](https://github.com/SitecorePowerShell/Console/issues/1109) to see the full scope of what changed. Any place where the name _Cognifide_ or _Cognifide.PowerShell_ was used is now replaced with _Spe_.
{% endhint %}

## Docker

Working with Docker is going to be the preferred method of installation. 

You can find two flavors of the images:
* [Community Built](https://hub.docker.com/r/sitecorepowershell/sitecore-powershell-extensions)
 * Ex: `docker pull sitecorepowershell/sitecore-powershell-extensions:6.4-1809`
* Sitecore Built
 * Ex: `docker pull scr.sitecore.com/sxp/modules/sitecore-spe-assets:6.4-1809`

With this approach you essentialy add a new layer during your image build to include the files from the asset image. Here are some samples of what you can add to your existing setup. Check out Sitecore's samples additional guidence.

**docker-compose.yml**

```text
services:
  mssql-init:
    image: ${REGISTRY}${COMPOSE_PROJECT_NAME}-xm1-mssql-init:${VERSION:-latest}
    build:
      context: ./docker/build/mssql-init
      args:
        BASE_IMAGE: ${SITECORE_DOCKER_REGISTRY}sitecore-xm1-mssql-init:${SITECORE_VERSION}
        SPE_IMAGE: ${SITECORE_MODULE_REGISTRY}sitecore-spe-assets:${SPE_VERSION}
  cm:
    image: ${REGISTRY}${COMPOSE_PROJECT_NAME}-xm1-cm:${VERSION:-latest}
    build:
      context: ./docker/build/cm
      args:
        BASE_IMAGE: ${SITECORE_DOCKER_REGISTRY}sitecore-xm1-cm:${SITECORE_VERSION}
        SPE_IMAGE: ${SITECORE_MODULE_REGISTRY}sitecore-spe-assets:${SPE_VERSION}
```

**Dockerfile (mssql-init)**

```text
# escape=`

ARG BASE_IMAGE
ARG SPE_IMAGE

FROM ${SPE_IMAGE} as spe
FROM ${BASE_IMAGE}

COPY --from=spe C:\module\db C:\resources\spe
```

**Dockerfile (cm)**

```text
# escape=`

ARG BASE_IMAGE
ARG SPE_IMAGE

FROM ${SPE_IMAGE} as spe
FROM ${BASE_IMAGE}

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

WORKDIR /inetpub/wwwroot

COPY --from=spe \module\cm\content .\
```

## Installation Wizard

The SPE module installs like any other for Sitecore. This approach is appropriate for installations not within a containerized environment.

[Download](https://github.com/SitecorePowerShell/Console/releases) the module from the GitHub releases page and install through the _Installation Wizard_.

## Troubleshooting

See the troubleshooting section [here](../troubleshooting.md)