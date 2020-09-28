# SSH Access

Everything you see below should be done on your RaspberryPI, via `ssh` or directly. To enable SSH access to your RaspberryPI, see the [official tutorial](https://www.raspberrypi.org/documentation/remote-access/ssh/).

It is recommmended to set a static IP address on the RaspberryPI for easier access. To set a static IP, do the following:

```bash
# Get the IPv4 address of the Raspberry
ip route get 1.2.3.4 | awk '{print $7}'
# Edit /etc/dhcpcd.conf
sudo nano /etc/dhcpcd.conf
# Modify the following lines (uncomment if necessary)
interface eth0
static ip_address=<YOUR_IPV4_ADDRESS>/24
static routers=192.168.0.1 # or 192.168.1.1 depending on your IPv4
static domain_name_servers=1.1.1.1
```

Save the modified file and restart your RaspberryPI. Check if the changes are saved by running the `ip route get 1.2.3.4 | awk '{print $7}'` command again on your RaspberryPI.

After this, it is recommended to set an alias on your host machine for easier SSH access. Place this in your `.bashrc` or `.zshrc`:

```bash
alias pi="ssh pi@<YOUR_IPV4_ADDRESS>"
```

Save and run `source ~/.bashrc` or `source ~/.zshrc` to reload the configuration. Now you will be able to SSH into your RaspberryPI by simply running `pi` on your host machine. To be able to log into your RaspberryPI without the need for a password, see the [official passwordless SSH guide](https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md).

> **_NOTE:_** If you are using `termite` as the terminal on your host machine, you might experience issues while SSH-ing. Place this line in the `~/.bashrc` or `~/.zshrc` file on you RaspberryPI: `export TERM=termite`.
