# k1s

> Lightweight k8s home lab cluster for Raspberry PIs.

## What is k1s

The K1S Platform contains all services required to start a k3d cluster on a Raspberry PI with Skaffold. It is meant for those who want to create their own home lab cluster with a lightweight Kubernetes release with a preloaded ingress controller and several monitoring tools. Since k1s is in the early stage of development, it currently allows only [Namesilo](https://www.namesilo.com/) as the DNS provider.

## Guide

Please follow this documentation from start to finish if you want to set k1s up on your Raspberry PI. It is still in the early development phase and unexpected bugs might occur if you skip some of the steps.

It is recommended to start this guide with the [Domain Setup](getting-started/dns).

## Features

k1s comes preloaded with the following:

- K3D
- Klipper ServiceLB
- Metrics Server
- Skaffold
- Traefik
- Prometheus
- Grafana
- Kubernetes Dashboard
- Hugo Dashboard UI
- GitHub Oauth2 Middleware
- Custom Error Pages Middleware
- Wildcard LetsEncrypt SSL Certificate
- DDNS Update CronJob for Namesilo A Records

The most of the setup is done via a CLI interface named [KubePI](https://github.com/nushkovg/kubepi). It is a custom tool for making the k1s management easier for the K3D setup, dependencies, submodules, and more. The usage of `kubepi` will be explained in more detail in the following sections.
