---
title: 'Upgrade Proxmox 7 to 8'
date: 2024-09-06T21:25:47-07:00
draft: false
hidden: false
externalURL: false
showDate: true
showModDate: true
showReadingTime: true
showTags: true
showPagination: true
invertPagination: true
showToC: true
openToC: false
showComments: false
showHeadingAnchors: true
tags: ['proxmox']
---

## Prerequisites

- Upgrade to the latest version of Proxmox VE 7.4
  - Ensure `web UI > Repositories` in pve-manager is at least version 7.4-15
- Valid and test backups for all VM’s and containers (good general rule for all time)
- At least 5GB of free disk space on the root mount point

## Upgrade

📌 Utilize the `pve7to8` checklist program (with the `--full` option) to provide hints and warnings about potential issues before and during the upgrade process

Update the configured APT repos and ensure system is on the latest package versions:

```bash
apt update
apt dist-upgrade
pveversion
```

The `pveversion` should display a value of at least `7.4-15` or newer!

Update the Debian base repo and Proxmox VE 8 Package repo:

- Debian base repo update to Bookworm
```bash
sed -i 's/bullseye/bookworm/g' /etc/apt/sources.list
```

- Proxmox VE 8 Package repo
```bash
sed -i -e 's/bullseye/bookworm/g' /etc/apt/sources.list.d/pve-install-repo.list
```

- For Enteprise subscriptions, do the following for repo updates
```bash
sed -i -e 's/bullseye/bookworm/g' /etc/apt/sources.list.d/pve-enterprise.list
```

{{< callout lightbulb >}}
If you have a subscription, uncomment the Enterprise subscriptions (remove the #)
{{< /callout >}}

After performing the above actions, refresh the repo package index: 
```bash
apt update
```

Upgrade the system to Debian Bookworm and Proxmox VE 8.0:

```bash
apt dist-upgrade
```

Check result and reboot into updated Kernel:

- Use the provided pve7to8 --full checklist command
```bash
pve7to8 --full
```

If the checklist returned any issues, you’ll be able to correct/fix them before moving on.

- Reboot system
```bash
shutdown -r now
```

Once everything has rebooted, login to your upgraded Proxmox VE 8.0 management console and confirm your environment/configurations!