# Ansible Role: Post-Install Checks

This Ansible role performs a series of post-installation validation checks to verify that a Linux server has been configured correctly after provisioning.

The role focuses on validating infrastructure components commonly required in production environments, including storage, networking, system performance, and swap configuration.

## Features

- Verifies encrypted storage devices
- Validates network interface configuration
- Checks bonded interface link speed
- Verifies system performance mode
- Confirms swap configuration
- Provides a concise validation summary

## Checks Performed

### Disk

- Detects encrypted (LUKS) devices
- Validates expected storage configuration

### Network

- Verifies required network interfaces
- Checks bond interface status
- Reports negotiated bond speed

### Performance

- Validates system performance profile/settings

### Swap

- Checks whether swap is configured and enabled

## Requirements

No additional Ansible collections are required beyond the built-in modules.

## Variables

Default values:

```yaml
infra_interfaces_to_check:
  - int1
  - int2
  - ext1
  - ext2
  - aggi
  - agge

infra_postinst_int_bond: aggi
infra_postinst_ext_bond: agge
```

Adjust these variables to match your environment.


## Example Output

```
TASK [Show validation summary]

ok: [server01] =>

Encrypted devices found:
- crypt_data
- crypt_logs

Speed of bond interface aggi: 25000 Mbps
```

## Typical Use Cases

- Post-installation validation
- Bare-metal server deployment
- Infrastructure acceptance testing
- Automated provisioning pipelines
- CI/CD infrastructure verification

## Notes

This role is intended to validate system configuration after deployment. It performs read-only checks and does not modify the target host.
