# auditd

## Overview

This module installs, configures and manages auditd. No default rules are
provided.

## Usage

### Defaults

Including `auditd` and using the defaults will;

- Install the audit daemon package
- Configure and manage both `/etc/audit/auditd.conf` and
`/etc/audisp/plugins.d/syslog.conf`
- Manage `/etc/audit/rules.d/audit.rules`
- Manage the file permissions of `/sbin/audispd`
- Enable and manage the `auditd` service

```puppet
include auditd
```

### Rules

The `audit::rule` define is used to create and manage auditd rules.

```puppet
auditd::rule { 'insmod':
  content => '-w /sbin/insmod -p x -k modules',
  order   => 10,
}

auditd::rule { '-w /var/run/utmp -p wa -k session': }
```

A hash can also be passed to the main `auditd` with the `rules` parameter:

```puppet
class { 'auditd':
  rules => {
    insmod => {
      content => '-w /sbin/insmod -p x -k modules',
      order   => 10,
    },
    sudoers_changes => {
      content => '-w /etc/sudoers -p wa -k scope',
      order   => 50,
    },
  },
}
```

With Hiera:

```yaml
auditd::rules:
  insmod:
    content: -w /sbin/insmod -p x -k modules
    order: 10
  sudoers_changes:
    content: -w /etc/sudoers -p wa -k scope
    order: 50
```
