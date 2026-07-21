# DHRU Fast Easy Cron

DHRU Fast Easy Cron is a secure one-file installer for running DHRU Fusion API cron operations on
a VPS. It replaces large collections of repeated cron URLs with one managed local system that
runs only the work DHRU currently needs.

## Main features

### Fast, load-aware processing

- Sends new API orders and retrieves supplier replies every **5 seconds**.
- Automatically works with **any active API that currently has active orders**.
- Does not require a separate send/get cron job for every API.
- Skips APIs without active orders, reducing unnecessary requests, overlap, and server load.
- Reads the current DHRU cron password automatically from the database on every run. If the
  password changes in DHRU, no cron URL or runner configuration needs to be edited.
- Spreads update-price work across six buckets at minutes `0,10,20,30,40,50`, so every active
  gateway is scheduled once per hour without one large price-update spike.
- Uses action-specific process locks to prevent overlapping duplicate executions.

### Portable installation

- Delivered as one executable `.run` file.
- Recursively discovers DHRU `configs/config.php` files and lets you select the correct installation
  when multiple copies exist.
- Supports common CyberPanel, cPanel, Plesk, and custom `/home` or `/var/www` layouts.
- Places its files outside the public webroot, beside `public_html` or `httpdocs` when possible.
- Supports root SSH, passwordless sudo, and website-user SSH installations according to the
  permissions available on the server.
- Detects Python 3.8+, required Python modules, MySQL client tools, package manager, systemd, user
  systemd, lingering, and crontab automatically.
- Skips legacy Python 3.6 commands and automatically selects a compatible versioned runtime such as
  `python3.9`; older dnf/yum systems can install `python39` automatically when authorized.
- Installs missing runtime dependencies when root or passwordless sudo is available, with support
  for `apt-get`, `dnf`, `yum`, `zypper`, `pacman`, and `apk` package managers.
- Uses systemd timers when available and a locked user-cron watchdog when necessary.

### Safe migration and upgrades

- Detects active legacy DHRU URL cron jobs and older runner jobs before installation.
- Offers automatic private backup and migration while preserving unrelated cron jobs.
- Prints a rollback command that restores the original crontab setup.
- Detects an existing or interrupted managed installation and requires approval before replacement.
- Creates a private timestamped backup and restores it automatically if replacement fails.
- Never overwrites an unknown installation directory.
- Runs configuration, database, encrypted-payload, dry-run, scheduler, and health checks before
  reporting installation success.

### Telegram monitoring and control

- Supports multiple private chats or groups.
- Each chat can independently select **All**, **Errors only**, or **Off** notifications.
- Detailed errors include the site, action, Fusion API name, gateway key, sanitized endpoint,
  duration, and response detail when available.
- Sends recovery notices and a 30-minute activity digest in All mode.
- Suppresses repeated identical errors to prevent a message every five seconds during an outage.
- Provides live **Cron Status** plus confirmed **Disable Cron** and **Enable Cron** controls.
- A paused system stops send, get, update-price, database, and DHRU API work while keeping only the
  lightweight Telegram control heartbeat alive so it can be enabled again.
- Only the configured private-chat owner or an administrator of a configured group can change
  settings. Pause and resume actions are recorded locally.

### Security and licensing

- Runs locally on your VPS; no free or paid external cron service is required.
- The runner payload is encrypted and is decrypted only after validating a signed activation.
- Activation is bound to the requested server identity and website installation.
- Supports publisher-issued lifetime or time-limited licenses.
- Stores configuration, state, backups, and rollback material with restrictive permissions.
- Redacts cron passwords and Telegram tokens from operational messages and logs.
- The public installer contains no signing private key, content key, activation generator, or
  plaintext runner source.
- Works with secure cPanel `/tmp` mounts using `noexec`; no insecure remount is required.

## Requirements

- A VPS or dedicated Linux server with SSH access.
- A DHRU Fusion website and readable `configs/config.php`.
- Python 3.8 or newer, installed automatically when root or passwordless sudo permits it.
- Root, passwordless sudo, or sufficient website-user permissions for one supported scheduler.
- Outbound HTTPS access when Telegram notifications are enabled.
- An activation supplied by the authorized distributor.

Shared hosting without SSH access is not supported.

## Fixed runtime policy

| Action | Schedule | Workers | Timeout | Retries |
|---|---:|---:|---:|---:|
| Send orders | Every 5 seconds | 2 | 10 seconds | 1 |
| Get replies | Every 5 seconds | 3 | 10 seconds | 1 |
| Update prices | Six 10-minute buckets | 1 | 60 seconds | 0 |

Send and get automatically target APIs with active orders. Update-price gateways are distributed
deterministically across the six buckets, giving each active gateway one scheduled update per hour.

## Fresh installation

Run these commands on the customer server:

```bash
git config --global http.version HTTP/1.1 && git clone https://github.com/markantony-sys/dhru-fast-easy-cron-public.git
cd dhru-fast-easy-cron-public
chmod 700 install-dhru-fast-easy-cron.run
./install-dhru-fast-easy-cron.run
```

The installer asks for the site name, lets you select the correct DHRU installation when needed,
optionally configures Telegram, displays the server-specific request key, and waits for the matching
activation key. Sensitive token and activation input is hidden.

Before enabling Telegram during installation, open the bot from every private recipient account
and press **Start**. For a group, add the bot and allow it to post messages.

## Update an existing checkout

```bash
git config --global http.version HTTP/1.1
cd ~/dhru-fast-easy-cron-public
git pull --ff-only origin main
chmod 700 install-dhru-fast-easy-cron.run
./install-dhru-fast-easy-cron.run
```

When upgrading an installed system, approve replacement only after the installer identifies it as
a managed DHRU Fast Easy Cron installation. The installer creates its private replacement backup
before changing the active installation.

## Telegram commands

| Command | Purpose |
|---|---|
| `/settings` | Open notification and cron controls |
| `/status` | Show notification mode and cron processing status |
| `/cronstatus` | Show cron processing status |
| `/help` | Show the control menu |

Cron Disable/Enable applies globally to the installed site. Notification mode applies only to the
chat that changes it.

## Manual and installer verification

Display the complete customer manual without installing:

```bash
./install-dhru-fast-easy-cron.run --manual
```

Current installer SHA-256:

```text
0fa349ce1a51c938653a619b60664c879e3b68c0c80292167353fd6d5047e25d
```

Verify it with:

```bash
sha256sum install-dhru-fast-easy-cron.run
```

Stop if the result does not match the trusted checksum above.

## Compatibility troubleshooting

- Keep `/tmp` mounted with `noexec` when that is the server policy. The installer invokes its
  extracted launcher safely through `sh`; do not remount `/tmp` with `exec`.
- If `/usr/bin/python3` is Python 3.6 but `python3.9` is installed, the installer automatically uses
  `python3.9`. You do not need to replace the operating system's Python alternative.

## License and support

An activation supplied by the authorized distributor is required. To get DHRU Fast Easy Cron,
contact [@chizlok on Telegram](https://t.me/chizlok).
