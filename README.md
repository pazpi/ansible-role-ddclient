maxhoesel.ddclient
=========

A very minimal role to install and configure ddclient from the GitHub source.
Also sets up a systemd service to enable daemon-mode for ddclient.

Requirements
------------

- A recent Ansible version. This role supports the 2 most recent major Ansible releases.
  Older versions might still work, but are not supported
- A host running:
  - Ubuntu 18.04 LTS or newer
  - Debian 10 or newer
  - Other distributions may work, but are not supported (Feel free to add support with a PR!)

Role Variables
--------------

### Installation

##### `ddclient_version`
- Version of ddclient to install (e.g. "3.9.1")
- See the [GitHub releases](https://github.com/ddclient/ddclient/releases) for the most recent version
- Default: `3.9.1`

##### `ddclient_executable_path`
- Where to put the ddclient executable
- Default is `/usr/local/sbin/ddclient`, as to not interfere with any distro-packages

##### `ddclient_configfile`
- Config file to use for the ddclient installation
- Default: `/etc/ddclient.conf`

##### `ddclient_pidfile`
- PID file to use for the ddclient daemon
- Default: `/var/run/ddclient.pid`

##### `ddclient_systemd_unit`
- Name of the unit file for the ddclient daemon
- Default: `ddclient`

### Configuration

##### `ddclient_interval`
- Number of seconds between DynDNS IP checks
- Default: `300`

##### `ddclient_mail`
- Send all updates to this user/mail address
- Default: `root`

##### `ddclient_mail_failure`
- Send all failures to this user/mail address
- Default: `root`

##### `ddclient_entries`
- List of ddclient configuration entries
- Each entry contains a list of options and a list of domains
  - Options map 1:1 to the ddclient parameters
- Example:
  ```yaml
  - options:
      protocol: cloudflare
      zone: domain.tld
      ttl: 60
      login: your-login-email
      password: APIKey
    domains:
      - domain.tld
      - my.domain.tld
  ```

### IP Lookup

##### `ddclient_use`
- Service/Method to use to determine the public IP address
- Options include `web, if, ip`
- Default: `web`

##### `ddclient_web`
- Web address to use for IP lookup when `ddclient_use = web`
- Default: `checkip.dyndns.org`

##### `ddclient_if`
- Interface to use for IP lookup when when `ddclient_use = if`
- Default: `eth0`

##### `ddclient_ip`
- IP to use for static lookup when `ddclient_use = ip`
- Default: `127.0.0.1`



Example Playbook
----------------

```yaml
- hosts: all
  tasks:
    - name: Install ddclient
      include_role:
        name: maxhoesel.ddclient
      vars:
        ddclient_entries:
        - options:
            protocol: cloudflare
            zone: domain.tld
            ttl: 60
            login: your-login-email
            password: APIKey
          domains:
            - domain.tld
            - my.domain.tld
```

License
-------

GPL 3 or later

Author Information
------------------

Created and maintained by Max Hösel (@maxhoesel)
