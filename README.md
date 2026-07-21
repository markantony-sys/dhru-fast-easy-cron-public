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
- Includes an optional Telegram bot menu for choosing detailed errors, errors plus recovery/digest,
  or disabled notifications independently in each chat.
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

The installer guides you through site detection, optional legacy-cron migration, Telegram chat
registration, activation, validation, and scheduler setup. Every chat starts in `all` mode and can
change its own setting later through the bot's `/settings` menu.

## Download verification

SHA-256:

```text
1d945a4623fc6ae548fe5ae6acc5bf82162b3b2ead427cc2b143ab511fd8c546
```

An activation supplied by the authorized distributor is required.
