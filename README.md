# Github action for CI-CD pipeline

## Overview

This document can be used to put in place your Release workflow as part of AirVantage project. Is goal is to create a new release or snapshot version of your application.

## Workflow setup

Copy template workflows from .github/workflow/release/ to .github/workflow/ and all files from .github/deploy/* to .github/deploy/ into your project.

Those files are to be modified for your project specific needs. See:
- [Customise workflow](#-Customise-workflow)
- [Customise deploy](#-Customise-deploy)

## Customise workflow

### Secrets

List of secrets needed for this workflow:
- **PAT_GITHUB_ACTION:** PAT for github action sharing
- **CONTAINERREGISTRY_USERNAME:** username to connect to container registry
- **CONTAINERREGISTRY_PWD:** pwd to connect to container registry
- **AWS_ACCESS_KEY_ID:** AWS access key id
- **AWS_SECRET_ACCESS_KEY:** AWS secret access key

### Triggers

- Manually started with a specific parameter :
    - **0** for normal release (default)
    - **1** for snapshot release.

- On merge on the branch master

### Steps

Some step might have to be updated depending on your project specifics

- Checkout: ref is the reference to the commit or release of the project **AirVantage/github-actions**.
Make sure you use the last tag reference in github-actions repository to date with this documentation

- Set up java: you might want to modify the java version please refer to this documentation
depending if your project is based on a [mvn build](https://github.com/actions/setup-java) (See 'Supported distributions' for available options)
or a [scala build](https://github.com/olafurpg/setup-scala) (See 'Usage' for available options)
- Set version: change the repo name
- Build docker : modify this step relatively to you project structure and name
- Push to s3: modify this step relatively to you project structure and name
- Push to registry: template contains both a push to Azure registry and ECR, you can remove one if no needs on your project. Modify this step relatively to you project structure and name.
- Save chart to registry: modify this step relatively to you project structure and name

Some specific actions might be required for your project before a specific step.
You can include them in the workflow between two steps. For example a redis:
```yml

      # Set projects specifics before step
      - name: Start Redis
        uses: supercharge/redis-github-action@1.2.0
        with:
          redis-version: 4
```

## Customise deploy
### Chart.yaml
The chart file contains apiVersion, the name and the type of the chart, description, namespace where the chart will deployed and the version
### Values.yaml
 We have one of this file for each environment: dev, stg, eu & na. This file contains 
### template
#### Service.yaml
 the templae folder contains kubernetes manifests which can have parameterized values






# Fluent Helm Charts

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Release Status](https://github.com/fluent/helm-charts/workflows/Release%20Charts/badge.svg?branch=main)](https://github.com/fluent/helm-charts/actions)

This functionality is in beta and is subject to change. The code is provided as-is with no warranties. Beta features are not subject to the support SLA of official GA features.

## Usage

[Helm](https://helm.sh) must be installed to use the charts.
Please refer to Helm's [documentation](https://helm.sh/docs/) to get started.

Once Helm is set up properly, add the repo as follows:

```sh
helm repo add fluent https://fluent.github.io/helm-charts
```

You can then run `helm search repo fluent` to see the charts.

## Contributing

We'd love to have you contribute! Please refer to our [contribution guidelines](CONTRIBUTING.md) for details.

## License

[Apache 2.0 License](./LICENSE).
