# ClamAV API CG App

## Head Start TTA Use

Branches of this repository:

* `main` - tracks updates from the [upstream repository](https://github.com/18F/clamav-api-cg-app)
* `dev` - OHS TTA customizations, including our deploy pipeline
* `ohs-deploy-dev` - Branch that gets auto-deployed to the `ttahub-dev` cloud.gov space
* `ohs-deploy-prod` - Branch that gets auto-deployed to the `ttahub-prod` cloud.gov space

Any custom work around deployment should branch off of `dev` before being merged and deployed.

## Why this project

This project aims to create a deployable cloud.gov app that will expose a REST api for scanning files for malware with ClamAV.

It is inspired by, and borrowed heavily from, https://blog.theodo.com/2017/11/implement-antivirus-api-10-min/

This manifest now runs a docker image from https://github.com/ajilaag/clamav-rest

## Setup

This project depends on one deployment variable, which is documented in `vars.yml-template`

`cp vars.yml-template vars.yml`

### Create app

To push the app to cloud.gov:

`cf push --vars-file vars.yml`

or to specify the project yourself:

`cf push --var project="PROJECT_NAME"`

### Configure networking

A [network policy](https://docs.cloudfoundry.org/devguide/deploy-apps/cf-networking.html#create-policies)
is required to route the TCP traffic from your app to the API endpoint on the apps.internal domain

`cf add-network-policy SOURCE_APP --destination-app DESTINATION_APP --protocol tcp --port 9443`

## Contributing

See [CONTRIBUTING](CONTRIBUTING.md) for additional information.

## Public domain

This project is in the worldwide [public domain](LICENSE.md). As stated in [CONTRIBUTING](CONTRIBUTING.md):

> This project is in the public domain within the United States, and copyright and related rights in the work worldwide are waived through the [CC0 1.0 Universal public domain dedication](https://creativecommons.org/publicdomain/zero/1.0/).
>
> All contributions to this project will be released under the CC0 dedication. By submitting a pull request, you are agreeing to comply with this waiver of copyright interest.
