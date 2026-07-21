# DHRU Fast Easy Cron

DHRU Fast Easy Cron is a one-file installer for running DHRU Fusion API cron operations quickly,
reliably, and without overlapping duplicate jobs.

Main features:

- Sends new API orders and retrieves supplier replies every five seconds.
- Automatically works with gateways that currently have active orders.
- Distributes price updates across six time buckets to reduce load spikes.
- Prevents overlapping executions with process locks.
- Supports common Linux hosting layouts and root or website-user SSH access.
- Includes Telegram failure notifications and automatic installation checks.
- Uses server-specific offline activation.

## Installation

Download `install-dhru-fast-easy-cron.run`, then run:

```bash
chmod 700 install-dhru-fast-easy-cron.run
./install-dhru-fast-easy-cron.run
```

Display the complete customer manual without installing:

```bash
./install-dhru-fast-easy-cron.run --manual
```

The installer guides you through site detection, Telegram configuration, activation, validation,
and scheduler setup.

## Download verification

SHA-256:

```text
93d9db21030b5dabd970fa43b2a3bc6f585096ecb370379e2d401639e647d2d1
```

An activation supplied by the authorized distributor is required.
