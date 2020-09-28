# Adding New Services

The platform is made with extensibility in mind, so that you can add your own services without too much hassle. This section explains how to do it.

> **_NOTE:_** You will probably need some knowledge about creating Kubernetes manifests and using Helm before adding a new service.

There are two ways to do this, by using submodules or simply doing it locally. The submodules way is **recommended** because it allows you to use version control and swap between version whenever you want by using the `git` CLI.

## The Submodules Way

Make sure you're in the root directory of the platform. Then add a new submodule from an existing repository. For example:

```bash
git submodule add git@github.com:nushkovg/k1s-example.git platform/k1s-example
```

This will create a new directory called `k1s-example` in the `./platform` directory. After doing this, run `kubepi platform init` to get the latest changes and synchronize the platform.

## The Local Way

To begin, create a new directory in the `./platform` directory. For example, create a `./platform/k1s-example` directory. In there, you can create any kind of service.

## Infrastructure Configuration

After using either the submodules or the local way of adding a service, you need to create the infrastructure manifests and initialize a new Helm Chart.

As the first step, create a new directory in `./infrastructure` with the same name as the directory for the service, for example `./infrastructure/k1s-example`.

### Creating the Chart

In the `./infrastructure/k1s-example` directory, create a `Chart.yaml` file with the following contents:

```yaml
name: k1s-example
version: 0.0.1
platform: k1s
description: K1S Example Helm Chart

```

After that, add a `.helmignore` file. You can just copy it from any other service in the `./infrastructure/k1s-example` directory.

### Adding the Kubernetes Manifests

In the `./infrastructure/k1s-example`, create another directory called `templates`. In here you can add any kind of a Kubernetes resource that your service requires. It is recommended that you follow the naming convention of the other services for the manifests.

After doing this, you should transform the manifests into Helm templates. To do this, you must add the new service and its values in `./infrastructure/helm-values.yaml`.

For example, add a new section like this at the bottom of the file:

```yaml
###############################
## K1S Example Configuration ##
###############################

example:
  name: example
  namespace: k1s
  replicas: 1

  ports:
    port: 443
    targetPort: 8443
    protocol: TCP
  
  image: # set automatically in skaffold
```

Whatever you do, **DO NOT** add the image name manually. Skaffold does this for you since it's building it itself.

After adding the values, replace the relevant fields in the manifests. Use the other services as a reference on how to do this if you're new to Helm.

## Editing the Skaffold Configuration

Once you've done everything, you are ready to add the new service to the `./skaffold.yaml` configuration.

### Adding the ARM build script

First, make sure to add the following bash script in the `./platform/k1s-example/scripts` directory, and name it `build.sh`:

```bash
#!/bin/bash

# Enable buildx features for cross-architecture builds
export DOCKER_CLI_EXPERIMENTAL=enabled

# Build the image
docker buildx build --platform linux/arm/v7 --tag $IMAGE .
```

After this, give the necessary permissions to the script:

```bash
chmod +x ./platform/k1s-example/scripts/build.sh
```

You will need this script because Skaffold needs to know how to build your service for an ARM architecture, which the RaspberryPI uses.

### Adding the service

In `skaffold.yaml`, add the following sections:

- Under `build.artifacts`:
  
  ```yaml
  - context: platform/k1s-example
    image: k1s-example
  ```

- Under `profiles.build.artifacts`:

  ```yaml
  - context: platform/k1s-example
    image: k1s-example
    custom:
      buildCommand: ./scripts/build.sh
    sync: {}
  ```

- Under `profiles.deploy.helm`:

  ```yaml
  - name: k1s-example
    namespace: k1s
    chartPath: infrastructure/k1s-example
    valuesFiles:
    - infrastructure/helm-values.yaml
    artifactOverrides:
      example.image: k1s-example
  ```

### Adding a DNS Record

In case your service has a UI, you can create a new DNS record for the subdomain as a CNAME, just like the other subdomains.

### Adding the Service to K1S Dashboard

If you'd like to add your own service to the k1s dashboard, you need to edit the `./platform/k1s-ui/data/links.yml` file and optionally add a logo. Here are the steps:

- In `./platform/k1s-ui/data/links.yml`, add the following at the bottom:

  ```yaml
  -
  name: "Example Service"
  url: "https://example-service.example.com"
  img: "logos/example-service.svg" # This is optional
  tags: ["monitoring"]
  ```

- Optionally, add a logo (preferably in a SVG format) in `./platform/k1s-ui/static/logos`.

### Testing the Service

After this, just restart the Skaffold process and test the functionality of the service. If something is wrong, check if you've missed a step. If you think there is something wrong with the platform itself, please open an issue on the GitHub repository.
