# Create Kubernetes Secrets

To be able to use the platform correctly and securely, you need to create the secret resources manually for the services that need them. The secrets are used for authorization on the different services and the use cases will be explained in this section.

## Namesilo API Key Secret

You need to create a secret containing the Namesilo API key. This is required for the LetsEncrypt and DDNS Update configurations to work. You can obtain the Namesilo API key by following these steps:

- Log into your [Namesilo](https://www.namesilo.com/) account
- Go in the [API Manager](https://www.namesilo.com/account/api-manager) section
- Generate a new API Key in the `API Key` section of the page
- Save the key somewhere safe

To create the secret, run the following command and replace the <API_KEY> with your own Namesilo API Key:

```bash
kubectl -n traefik create secret generic namesilo \
        --from-literal='NAMESILO_API_KEY=<API_KEY>'
```

## Grafana Admin Password Secret

You need to set a default password to be able to log into the Grafana Dashboard. You can choose any password you'd like and create the secret with it:

```bash
kubectl -n monitoring create secret generic grafana \
        --from-literal='GF_SECURITY_ADMIN_PASSWORD=<YOUR_PASSWORD>'
```

## GitHub Oauth2 Secret

Before creating the secret, you have to create an application on your GitHub account. You only need to register an "OAuth Application" (as opposed to a full "Github Application"), which you can do [here](https://github.com/settings/applications/new). The fields should be populated like this:

- Application name: `k1s`
- Homepage URL: `https://example.com`
- Application description: `Lorem ipsum description.`
- Authorization callback URL: `https://oauth.example.com/_oauth`

Replace `example.com` with your domain both in the homepage URL and the authorization callback URL.

After creating the application, GitHub will create a `Client ID` and `Client Secret` for the application. Save them somewhere safe, you will need them for creating the secret.

After this, you just need to create a random string which will be used to to sign cookies authentication. To do this, run the following, and save the output somewhere safe:

```bash
openssl rand -hex 16
```

After everything is ready, you can create the secret:

```bash
kubectl -n traefik create secret generic traefik-forward-auth \
        --from-literal='PROVIDERS_GENERIC_OAUTH_CLIENT_ID=<APP_CLIENT_ID>' \
        --from-literal='PROVIDERS_GENERIC_OAUTH_CLIENT_SECRET=<APP_CLIENT_SECRET>' \
        --from-literal='SECRET=<RANDOM_STRING>'
```
