# Tang snap

Tang is a server for binding data to network presence.

This is the [reference server](https://github.com/latchset/tang) packaged as a [snap](https://snapcraft.io) container.

Due to current limitations, the daemon is configured as a standard service in listening mode with privileges dropped to the special `snap_daemon` user/group. This restricts the server port to 1024 or above. Without further configuration **the default port is 8080**.

## Requirements

Currently only Ubuntu 23.04 amd64 build environment has been tested. Cross-compiling for ARM is planned for the future, but currently neither tested nor configured. *Unverified* requirements include at least Snapcraft 7.0 and snapd 2.45.

This snap requires the following interfaces, which are automatically connected:
* network
* network-bind

After install you can run `snap connections tang` to verify this.

## Install from Snap Store

TODO not available yet

## Build from source

To build the snap locally, after installing and configuring [snapcraft](https://snapcraft.io/docs/snapcraft-overview):
```
git clone https://github.com/salix-caprea/tang-snap
cd tang-snap
snapcraft --debug
```

Tests are automatically run as part of the build step. Add `--verbosity=debug` to view build output.

To install the snap:
```
snap install tang_14-0.1_amd64.snap --devmode
```

You should see the service running and listening on the default port with e.g.
* `systemctl status snap.tang.tangd`
* `journalctl -xef -u snap.tang.tangd`
* `ss -np state listening '( sport = :8080 )'`

For a basic test of the tangd daemon:
```
curl 127.0.0.1:8080/adv
```
This command should return a response with a set of initial public keys. See upstream documentation for API details.

