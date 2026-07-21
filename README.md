# DHRU Fast Easy Cron

DHRU Fast Easy Cron is a one-file installer for running DHRU Fusion API cron operations quickly,
reliably, and without overlapping duplicate jobs.

Main features:

- Sends new API orders and retrieves supplier replies every five seconds.
- Automatically works with gateways that currently have active orders.
- Distributes price updates across six time buckets to reduce load spikes.
- Prevents overlapping executions with process locks.
- Supports common Linux hosting layouts and root or website-user SSH access.
- Detects old DHRU cron jobs, offers private automatic backup/migration, and generates a rollback
  command while preserving unrelated cron jobs.
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

The installer guides you through site detection, optional legacy-cron migration, Telegram
configuration, activation, validation, and scheduler setup.

## Download verification

SHA-256:

```text
42df8949d8d8f1ce56bf9327de7a0e6a26b318b7d3f60efa8818c4930ca00518
```

An activation supplied by the authorized distributor is required.
