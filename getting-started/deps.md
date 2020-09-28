# Dependencies

You will need the following packages installed on your RaspberryPI:

- git
- go
- docker
- python3-pip

### Raspberry Pi OS (Raspbian)

```bash
sudo apt-get install python3-pip git docker.io golang-go
```

### Arch Linux

```bash
sudo pacman -Syu python3-pip git docker go
```

### Docker Setup

Add your user to the `docker` group and log-off/log-on from your X session, reboot, or just type in `newgrp docker`.

```bash
sudo usermod -aG docker $USER
```

### Clone k1s

```bash
git clone git@github.com:nushkovg/k1s.git
cd k1s
```
