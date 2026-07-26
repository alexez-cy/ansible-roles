# Ansible Role: Encrypted LVM Storage with LUKS

This Ansible role automates the provisioning of encrypted storage volumes using **LUKS2**, **LVM**, and a configurable filesystem.

## Features

- Installs required encryption and storage packages
- Creates and configures LUKS2 encrypted volumes
- Opens encrypted devices automatically
- Creates LVM volume groups and logical volumes
- Creates filesystems on logical volumes
- Configures persistent unlock via `/etc/crypttab`
- Mounts encrypted storage automatically
- Applies cryptsetup performance tuning parameters
- Designed to be idempotent and safe for repeated runs

## Supported Components

- LUKS2 encryption
- LVM2
- ext4/xfs and other supported Linux filesystems
- Debian/Ubuntu
