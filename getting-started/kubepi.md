# KubePI

For easier usage and setup there is a CLI called [KubePI](https://github.com/nushkovg/kubepi). It is a custom tool for making the k1s management easier for the K3D setup, dependencies, submodules, and more. It needs at least Python 3.7. Please check that you have Python 3.7 or later by running:

```bash
# Raspberry Pi OS (Raspbian) users
python3 --version
# Arch users
python --version
```

If you are not running Python 3.7 or later, please upgrade your Python version.

[KubePI](https://github.com/nushkovg/kubepi) is available as an official [PyPI package](https://pypi.org/project/kubepi/). Follow these steps to install `kubepi` by using `pip`:

```bash
# Raspberry Pi OS (Raspbian) still shipping python2 by default
pip3 install kubepi
# Arch shipping python3 by default
pip install kubepi
```

Now you should have the `kubepi` command in your path and you can run `kubepi` to display the help:

```bash
pi@k1s:~$ kubepi
Usage: kubepi [OPTIONS] COMMAND [ARGS]...

  Kubepi CLI for easier k3d setup on Raspberry PI.

Options:
  --kube-context TEXT  The kubernetes context to use
  -v, --verbosity LVL  Either CRITICAL, ERROR, WARNING, INFO or DEBUG
  --help               Show this message and exit.

Commands:
  apps       App commands
  k3d        Manage k3d clusters
  platform   Platform commands
  preflight  Preflight checks
  setup      Setup infrastructure services
```

To see the options of each command, just run:

```bash
kubepi <COMMAND> --help
```

For example:

```bash
pi@k1s:~$ kubepi k3d --help
Usage: kubepi k3d [OPTIONS] COMMAND [ARGS]...

  Manage k3d clusters. The name of the cluster is taken from the option
  --kube-context which defaults to 'k1s'

Options:
  --help  Show this message and exit.

Commands:
  create  Create cluster
  delete  Delete cluster
  start   Start cluster
  status  Cluster status
  stop    Stop cluster
```
