# Tailscale on OpenWRT

# DEPRECATED!

This repositoy is now deprecated and no longer supported! Please use [lanrat.github.io/openwrt-tailscale-repo](https://lanrat.github.io/openwrt-tailscale-repo) instead.

# Installation

1. Install the prerequisites for wget and tailscale:

```sh
opkg update
opkg install libustream-openssl ca-bundle kmod-tun
```


2. Install the service and download the latest Tailscale binaries.
set `install_path` if you want to have a persistant install, otherwise defaults to `/tmp`.

```sh
export install_path="/mnt/sda1/tailscale"
wget -O- 'https://raw.githubusercontent.com/lanrat/openwrt-tailscale-enabler/main/bin/tailscale-install' | sh
```

3. Run tailscale for the first time:

```sh
/etc/init.d/tailscale start
tailscale up --accept-dns=false--advertise-routes=10.0.0.0/24 --advertise-exit-node
```

4. Enable tailscale at boot:

```sh
/etc/init.d/tailscale enable
```

Verify by looking for an entry here:

```sh
ls /etc/rc.d/S*tailscale*
```

5. Reboot the router and verify that it shows up online on the [Tailscale Admin portal](https://login.tailscale.com/admin/machines).

6. To update the version of tailscale, run `tailscale-install` or run step 2 again.

Note: You need to have at least ~27 MB of free space in `$install_path` to be able to use this.


## Uninstallation

```shell
/etc/init.d/tailscale stop
/etc/init.d/tailscale disable
rm -rf /usr/bin/tailscale /usr/bin/tailscaled /etc/init.d/tailscale /usr/bin/tailscale-install /etc/config/tailscale.state
```
