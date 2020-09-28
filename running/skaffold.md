# Skaffold

Skaffold is a command line tool that facilitates continuous development for Kubernetes applications. You can iterate on your application source code locally then deploy to local or remote Kubernetes clusters. Skaffold handles the workflow for building, pushing and deploying your application. It also provides building blocks and describe customizations for a CI/CD pipeline.

For more information about how Skaffold works, check the [official documentation](https://skaffold.dev/docs/).

## Starting the Workflow

To start the `k1s` workflow, use the `skaffold` CLI.

There are several ways that you can start the workflow. The following command is the **recommended** one, because the `--cleanup=false` flag allows every Kubernetes resource to stay persistent and not get deleted upon stopping the `skaffold` process:

```bash
skaffold dev --port-forward --force=false --cleanup=false
```

You can also start the workflow with the `--cleanup=true` flag, which is **not recommended** except when adding a new service to the platform and you don't care about persistance in the cluster:

```bash
skaffold dev --port-forward --force=false --cleanup=true
```

If you don't want the service logs to appear, just add the `--tail=false` flag to the command.

The first time you're running Skaffold on your RaspberryPI might take around 5 minutes, depending on your RaspberryPI version. This is because it is building the submodules and it's pulling the required Docker images for each service.

Once everything is done and Skaffold is up, you can check the status of the pods:

```bash
kubectl get pods --all-namespaces -w
```

Skaffold allows you to make changes to the existing services, or add new ones, without restarting the cluster. This means that it is watching for file changes and automatically rebuilds the services if a change is saved in any service.

## Authentication

Your domain is protected with the Oauth2 setup provided by GitHub. And since you've set a whitelist in the configuration, only you can access your own home cluster.

Once you've logged in, the browser saves an authentication cookie which allows you to use the platform without relogging each time.

If you want to add other users, create another whitelist environment variable with the same name `WHITELIST` in the `k1s-oauth` deployment and set the other user's email in the helm charts by following the same principles.

For more information about environment variables, check [thomseddon's](https://github.com/thomseddon) guide about additional [configuration](https://github.com/thomseddon/traefik-forward-auth#configuration).

## LetsEncrypt SSL Certificate

If you chose the production server in the configuration, you should have a free SSL certificate set up on your domain and on the subdomains, because of the wildcard certificate.

Since the DNS propagation is really slow on Namesilo, it might take up to 40 minutes for a certificate to be generated, so just let it create itself without worrying that it's not working. You can still use everything on the platform while a certificate is being generated.
