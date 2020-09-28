# Getting a Domain

First of all, you will need a domain bought from Namesilo. As mentioned previously, it is the only supported DNS registrar in this early release of k1s. New DNS registrar options will come in the next releases, but for now you can only use Namesilo.

## DNS Records

You need to prepare the DNS records once you've gotten your domain. Follow these steps to do this:

- Log into your [Namesilo](https://www.namesilo.com/) account
- Go to the [Account domains](https://www.namesilo.com/account_domains.php) page
- Select your domain from the list
- Select **Update** on the **DNS Records** field from the Domain Console list
- Create the DNS records

Here is the list of the required records:

| HOSTNAME   | TYPE  | ADDRESS/VALUE      | DISTANCE/PRIO | TTL  | SERVICE   |
|------------|-------|--------------------|---------------|------|-----------|
|            | A     | \<YOUR_EXTERNAL_IP> | NA            | 3603 | 3rd-party |
| www        | CNAME | example.com        | NA            | 3603 | 3rd-party |
| traefik    | CNAME | example.com        | NA            | 3603 | 3rd-party |
| oauth      | CNAME | example.com        | NA            | 3603 | 3rd-party |
| grafana    | CNAME | example.com        | NA            | 3603 | 3rd-party |
| kube       | CNAME | example.com        | NA            | 3603 | 3rd-party |
| prometheus | CNAME | example.com        | NA            | 3603 | 3rd-party |

Make sure to replace `<YOUR_EXTERNAL_IP>` with your external IP address. You can find it out via [WhatIsMyIP](https://www.whatismyip.com/). Also replace the `example.com` with your own domain.

The DDNS CronJob which is integrated into k1s will take care of updating the IP address of the A record each time it changes (in case of a dynamic IP), so you won't have to do it manually.

## Router Port Forwarding

After making sure that you have a domain registered, you have to open port `:80` and port `:443` on your network by using the port forwarding option on your router. Since there are many different routers out there, you must find out for yourself about how to do this on your own router.

You can check if the ports are open [here](https://www.canyouseeme.org/).

After getting a domain and opening the required ports, you can continue with this guide.
