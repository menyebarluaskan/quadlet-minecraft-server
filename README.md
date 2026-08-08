# Quadlet Minecraft Server Template

This is my takes on containerized minecraft setup. It is using
[itzg/minecraft-server](https://github.com/itzg/docker-minecraft-server) image.
**NOTICE: This is my personal configuration using geyserMC.** It is also
using tailscale for local deployment that enables limited remote access. This
also using rootless container configuration.

## Requirements

* Compatible Linux/Unix OSes that are using systemd (Debian, Ubuntu, Fedora,
  Arch, etc.)
* Podman

## Deployment

Copy the environment variables example files and fills them with your own
data / configuration.

* `quadlet/minecraft-server.env.example`
* `quadlet/minecraft-tailscale.env.example`

Copy the volumes configuration example files and fills them with your own
configuration too.

* `quadlet/minecraft-server.container.d/01-server-volume.conf.example`
* `quadlet/minecraft-tailscale.container.d/01-tailscale-volume.conf.example`

Add Tailscale Auth Key into podman secret using the following command:

```bash
echo "<your ts auth key here>" | podman secret create ts_authkey -
```

Ensure that linger for user is enabled. This ensure that the minecraft server
can be loaded after boot up

```bash
sudo loginctl enable-linger $USER
```

check if the linger is already enabled using the following command.

```bash
loginctl show-user $USER --property=Linger
```

Output:

```bash
Linger=yes
```

Then copy or link the contents of `quadlet/` into the following directory
`$HOME/.config/containers/systemd` or create a subdirectory such as
`$HOME/.config/containers/systemd/minecraft-server`.

Then run the following command.

```bash
systemctl --user daemon-reload
systemctl --user start minecraft-server-pod.service
```

## Configuration

The general minecraft server configuration can refer to the original
project's [documentation](https://docker-minecraft-server.readthedocs.io/) that
are using environment variables thus it is stored in
`quadlet/minecraft-server.env`
