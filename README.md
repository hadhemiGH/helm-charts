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
### Chart
The Chart.yaml file is required for a chart. It contains the following fields:
- ApiVersion: The chart API version 
- Name: The name of the chart
- Type: The type of the chart
- Version: The targeted Helm chart version
- Namespace: The cluster namespace where the chart will deployed 
### Values
Contains the values that need to be used for the parameterized variables in the template for a specific deployment. In Lwm2mfe application, we have one of this file for each environment: dev, stg, eu & na. This file contains:
- ReplicaCount: number of replicas for author and public pods, the default is 1
- Resource Quota: provides constraints that limit aggregate resource consumption per namespace Resource limits describe how many resources, for example, CPU or RAM, a container can use.
- Resources
    - **Resource requests** describe how many resources, for example, CPU or RAM a node has to have
    - **Resource limits** describe how many resources, for example, CPU or RAM, a container can use.
- Image: container image repository and tag for lwm2mfe application the docker image will added dynamically when the release script executed  
- EnvName: The environment name (dev, stg, eu or na)
### Template
The most important ingredient of a chart is the templates/ directory. It holds the application’s configuration files that will be deployed to the cluster. The template files fetch deployment information from values.yaml.
templates/ directory contains 
#### Configmap
ConfigMap allows you to decouple an environment-specific configuration from pods and containers. It stores data as key-value pairs that can be consumed in other places. Config map used in this application as a pod volume that is mounted to containers.
#### Deployment 
In templates/deployment.yaml, I have added a new tier label to the deployment’s metadata for more flexibility in deployment and lookup.
This file includeimportant parameters such as the replica ,selector, container image and ports, liveness and readiness probes:

In addition to our tier label being added to the auto-generated deployment template, we have added:
- env field inside the containers spec which allows us to add the ConfigMapkey . 
- volumeMounts: part of a Kubernetes Pod spec that describes how and where a volume is mounted within a container.
- liveness and readiness: probes that K8S uses to determine if the application is ready to accept requests or needs to be restarted
#### _helpers
A place to put template helpers that can be re-used throughout the chart. In this file, we added a common labels such as Component, Application, EnvName and the Name




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
