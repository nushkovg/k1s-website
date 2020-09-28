# Installation

Follow the steps carefully to be able to install everything necessary for running k1s.

## Tools Setup

The platform uses the following tools. They can be automatically installed with `kubepi`:

- kubectl
- helm
- k3d
- skaffold

Use `kubepi` to initialize the toolchain. The command below will download all necessary tools and store them in the `./bin` directory. Make sure you are in the root of the cloned git repository and run:

```bash
kubepi setup init
```

Follow the instructions prompted to add the bin/ directory to your PATH. Make sure to log-off/log-on once you have done so. If you don't want to do that, simply run `source ~/.profile`, `source ~/.bashrc` or `source ~/.zshrc`.

> **_NOTE:_** The command will tell you to place the directory in your PATH by placing it in the `~/.profile` file. If for some reason this does not work, replace `~/.profile` with `~/.bashrc` or `~/.zshrc` in the `echo export PATH=<K1S_DIRECTORY>/bin:$PATH >> ~/.profile` command and run `source ~/.bashrc` or `source ~/.zshrc` again.

## Environment Setup

Make sure that you are in the root of the repository. There are certain defaults that are in use:

- The default context is set to **k1s**
- The k3d cluster is created and sets the kube-context to **k1s**
- **k1s** namespace is created
- **traefik** namespace is created
- **monitoring** namespace is created

The `kubepi` CLI provides a `preflight` command which checks if everything is in order and every dependency is installed correctly:

```bash
kubepi preflight
```

If `preflight` returned that everything is in order, proceed with the next steps.

First, run the following command which initializes the platform by pulling every submodule required in the `./platform` directory and synchronizes the repositories and its branches. More on using the submodules will be explained later.

```bash
kubepi platform init
```

If the platform is successfully initialized, proceed with creating the cluster:

```bash
kubepi k3d create
```

If everything ran without any errors, the cluster is ready. To check the status of the master node:

```bash
kubectl get nodes -o wide
```

To list the available pods:

```bash
kubectl get pods --all-namespaces -w
```

## Managing `k3d`

k3d is running Kubernetes as Docker containers on your Docker daemon. These Docker containers are stopped when you restart Docker or your RaspberryPI.

Make sure that `k3d` is running:

```bash
kubepi k3d status
```

If `k3d` is up and running you can move to the next section. If it's not running, you can start it with:

```bash
kubepi k3d start
```

If you'd like to stop k3d you can run:

```bash
kubepi k3d stop
```
