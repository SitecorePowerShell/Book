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

With this approach, you essentially add a new layer during your image build to include the files from the asset image. Here are some samples of what you can add to your existing setup. Check out Sitecore's samples for additional guidance.

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

* For Sitecore 10.1 and newer along with Identity Server you should enable the provided configuration _Spe.IdentityServer.config_.
* For Sitecore 10.1 and newer you can leverage the IAR packages. There is still the need for *dacpac* deployments because SPE includes a security user and role.
  * An additional patch is required for 10.1 to include the location `/sitecore modules/items/`. See [Gist here](https://gist.github.com/michaellwest/87c26d25407b0a8bcfbcabfabedbbdb7).



## Upgrade

We've tried to make upgrading SPE as seamless as possible. The following should provide you with some basic information on what to expect when upgrading.

### From 6.x

You should be able to install directly over the previous installation of 6.0+.

* For Sitecore 10.1 and newer along with Identity Server you should enable the provided configuration _Spe.IdentityServer.config_.
* For Sitecore 10.1 and newer you can leverage the IAR packages. There is still the need for *dacpac* deployments because SPE includes a security user and role.
  * An additional patch is required for 10.1 to include the location `/sitecore modules/items/`. See [Gist here](https://gist.github.com/michaellwest/87c26d25407b0a8bcfbcabfabedbbdb7).

{% hint style="info" %}
Using packages with the *Items as Resources* (IAR) files will require additional steps to remove database records that correlate with the contents of the _*.dat_ files. Refer to the Sitecore documentation when upgrading for the process to cleanup the databases. [This post](https://www.maartenwillebrands.nl/2022/02/15/sitecore-removing-iar-items-from-the-database/) may be helpful if you wish to take a more precise approach.
{% endhint %}

### From 5.x and older

These versions used a different name for the assemblies and configs. You may find it easier to delete all files originally distributed with SPE before installing a newer version. Here are a few steps to consider:

* Delete all files in the _bin_ and _App_Config/Include_ directories prefixed with *Cognifide*.
* Delete _/sitecore modules/Shell/PowerShell_
* Delete _/sitecore modules/PowerShell_

Reference the original SPE package (if available) to identify if any other files were included.

Sitecore items in the content tree will potentially shuffle around into their new location. Be sure to backup your custom scripts first.

## Troubleshooting

See the troubleshooting section [here](../troubleshooting.md)
