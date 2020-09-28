# Accessing k1s

k1s provides you with several dashboards which you can access via your domain and subdomains. This section explains how to access them.

## K1S Dashboard

Just visit your main domain. For example: `https://example.com`.

## Traefik Dashboard

Open it via the k1s dashboard or visit the subdomain you've created. For example: `https://traefik.example.com`.

## Grafana Dashboard

Open it via the k1s dashboard or visit the subdomain you've created. For example: `https://grafana.example.com`.

To log into the dashboard, use the username and password you've created in the secrets and the helm values configuration.

## Prometheus Dashboard

Open it via the k1s dashboard or visit the subdomain you've created. For example: `https://prometheus.example.com`.

## Kube Dashboard

Open it via the k1s dashboard or visit the subdomain you've created. For example: `https://kube.example.com`.

To get into the dashboard, you will need the platform token. You can get the token by using the relevant `kubepi` command:

```bash
kubepi platform token
```

## Pulling Changes

When pulling changes with `git pull` and you're using submodules, you must run the following command to pull the submodule changes and synchronize the platform again:

```bash
kubepi platform init
```

## Getting the Platform URL

To get the main URL of the platform, which is the K1S Dashboard, you can run:

```bash
kubepi platform info
```

## Checking Submodule Versions

To get the current submodule branches and commit hashes, run:

```bash
kubepi platform version
```

## Checking the Logs Manually

If you want to check the logs of a particular service, you can do the following in a separate terminal window:

- Check the names and the namespaces of the running pods:

  ```bash
  kubectl get pods --all-namespaces
  ```

- Get the logs:

  ```bash
  kubectl logs -f -n <NAMESPACE> <POD_NAME>
  ```
