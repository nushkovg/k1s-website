# Container Features

You need to enable the container features in the kernel in order to run the platform. If you have already done this on your Raspberry PI, then you can skip this step.

Edit the file `/boot/cmdline.txt`:

```bash
sudo nano /boot/cmdline.txt
```

and add the following parameters **at the end** of the line:

```bash
cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory
```

After this, save the file.

## Firewall

It is also recommended to switch the Debian firewall to its legacy configuration:

```bash
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
```

You can now restart your Raspberry PI and continue with this guide.
