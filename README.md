# ClamAV API CG App

## Head Start TTA Use

Branches of this repository:

* `main` - tracks updates from the [upstream repository](https://github.com/18F/clamav-api-cg-app)
* `dev` - OHS TTA customizations, including our deploy pipeline
* `ohs-deploy-dev` - Branch that gets auto-deployed to the `ttahub-dev` cloud.gov space
* `ohs-deploy-prod` - Branch that gets auto-deployed to the `ttahub-prod` cloud.gov space

Any custom work around deployment should branch off of `dev` before being merged and deployed.

CircleCI pipelines can be found [here](https://app.circleci.com/pipelines/github/HHS/Head-Start-clamav-api-cg-app).  Push branch to remote, then trigger deploy job targeting dev with your branch.  Merge changes into `dev`, then `ohs-deploy-dev`, then `ohs-deploy-prod`

## Description

This project aims to create a deployable cloud.gov app that will expose a REST api for scanning files for malware with ClamAV.  The docker container runs an underlying freshclam service and REST API server.  ClamAV file definitions will be automatically updated from an upstream server.

This manifest runs a docker image from [ajilaag/clamav-rest](https://hub.docker.com/r/ajilaag/clamav-rest)

## Troubleshooting

*Error code 403 from the ClamAV Content Delivery Network (CDN)*

This is likely due to running an out-of-date version of the freshclam service.
Rebuild on the latest docker image and try redeploying the application.

## Initial Setup

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
