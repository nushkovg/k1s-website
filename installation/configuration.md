# Required Configuration

Before finally running the platform, you need to configure the correct values in several files.

## K1S Dashboard

k1s comes with a preloaded dashboard which is in fact, a submodule located in the `./platform` directory, as the other submodules. You just need to set your domain in its configuration.

- In `./platform/k1s-ui/config.toml`, replace the following with your own domain:

  ```toml
  baseURL = "https://<YOUR_DOMAIN>/"
  ```

- In `./platform/k1s-ui/data/links.yml`, replace the following with your own domain:

  ```yaml
  # Traefik
  url: "https://traefik.<YOUR_DOMAIN>"
  # Grafana
  url: "https://grafana.<YOUR_DOMAIN>"
  # Kubernetes
  url: "https://kube.<YOUR_DOMAIN>"
  # Prometheus
  url: "https://prometheus.<YOUR_DOMAIN>"
  ```

## Required Helm Values

Since k1s is using Helm, the configuration is done in a values YAML file. It is not recommended changing any values which are not mentioned in this section, but if you do, do it on your own risk.

The keys which need configuration are marked with a `# CONFIGURE ME` comment. To configure the values, open `./infrastructure/helm-values.yaml` and set the following values, and make sure you copy/paste them as written in the Value column:

### K1S UI

| Key             | Value                | Description                  |
|-----------------|----------------------|------------------------------|
| ui.ingress.rule | Host(\`\<YOUR_DOMAIN>`) | The ingress rule for the UI. |

### Traefik

| Key                        | Value                                          | Description                                                       |
|----------------------------|------------------------------------------------|-------------------------------------------------------------------|
| traefik.args.le.email      | \<YOUR_NAMESILO_EMAIL_ADDRESS>                  | The email address you've used to register a domain on Namesilo.   |
| traefik.args.le.caServer   | https://acme-v02.api.letsencrypt.org/directory | LetsEncrypt production server for generating an SSL certificate.  |
| traefik.args.le.mainDomain | "*.\<YOUR_DOMAIN>"                              | Your Namesilo domain for which a wildcard certificate is created. |
| traefik.args.le.sanDomain  | "\<YOUR_DOMAIN>"                                | The same domain as the main domain.                               |
| traefik.ingress.rule       | Host(\`traefik.<YOUR_DOMAIN>`)                  | The ingress rule for the Traefik dashboard.                       |

> **_NOTE:_** The LetsEncrypt production server can only generate a certificate 5 times per week. The configuration for `traefik.args.le.caServer` above is for the production server, but if you need to test something while developing a new service, use the staging server which allows for infinite certificates: `https://acme-staging-v02.api.letsencrypt.org/directory`.

### GitHub Oauth2

| Key                            | Value                        | Description                                                             |
|--------------------------------|------------------------------|-------------------------------------------------------------------------|
| oauth.env.whitelist.value      | \<YOUR_GITHUB_EMAIL_ADDRESS>  | The email address on your GitHub account.                               |
| oauth.env.logoutRedirect.value | https://<YOUR_DOMAIN>        | Redirection URL upon logout. Set your main domain.                      |
| oauth.env.cookieDomain.value   | \<YOUR_DOMAIN>                | The domain on which the auth cookie is saved. Must be your main domain. |
| oauth.env.authHost.value       | oauth.\<YOUR_DOMAIN>          | The Oauth subdomain which you created in the DNS records.               |
| oauth.ingress.rule             | Host(\`oauth.\<YOUR_DOMAIN>`) | The ingress rule for the Oauth2.                                        |

> **_IMPORTANT:_** The whitelist email means that only you can access your own platform. However, as per GitHub's documentation, their `/user` endpoint only returns the user's email if it's publicly visible. As such, you MUST set your email to be public on GitHub.

### Prometheus

| Key                     | Value                             | Description                                    |
|-------------------------|-----------------------------------|------------------------------------------------|
| prometheus.ingress.rule | Host(\`prometheus.\<YOUR_DOMAIN>`) | The ingress rule for the Prometheus dashboard. |

### Grafana

| Key                    | Value                          | Description                                   |
|------------------------|--------------------------------|-----------------------------------------------|
| grafana.env.user.value | \<ANY_USERNAME_YOU_WANT>        | The admin username for the Grafana dashboard. |
| grafana.ingress.rule   | Host(\`grafana.\<YOUR_DOMAIN>`) | The ingress rule for the Grafana dashboard.   |

### Kubernetes Dashboard

| Key                    | Value                      | Description                                    |
|------------------------|----------------------------|------------------------------------------------|
| dashboard.ingress.rule | Host(\`kube.\<YOUR_DOMAIN>`) | The ingress rule for the Kubernetes dashboard. |
