ansible-role-shelly
====

Configure Shelly-NG (second generation) devices with curl over RPC API.

Requirements
------------

* Shelly-NG (tested on Shelly Plus 2 PM).
* curl

Role Variables
--------------

* `shelly_username:` - Device username.
* `shelly_password` - Device password.
* `shelly_curl_auth` - Authentication part of `curl`.
* `shelly_rpc_uri` - RPC URI.
* `shelly_rpc_set_config` - dict with configuration for `Sys.SetConfig` method. Check [official documentation](https://shelly-api-docs.shelly.cloud/gen2/ComponentsAndServices/Sys/#configuration) for available settings. Example:
```yaml
shelly_rpc_set_config:
  device:
    name: "{{ inventory_hostname }}"
    profile: cover
  location:
    tz: Europe/Prague
  sntp:
    server: time.nist.gov
```
* `shelly_rpc_set_config_combined` - dict with complete configuration for `Sys.SetConfig`.
* `shelly_rpc_get_status` - dict to call `Sys.GetStatus` method.
* `shelly_rpc_reboot` - dict to call `Shelly.Reboot` method.
* `shelly_restart` - boolean to reboot Shelly if required.

Dependencies
------------

Collections:

* `ansible.builtin`


Example Playbook
----------------

```yaml
- hosts: my_shelly_devices
  gather_facts: no
  vars:
    shelly_password: verysecure
    shelly_rpc_set_config:
      device:
        name: "{{ inventory_hostname }}"
        profile: cover
      location:
        tz: Europe/Prague
      sntp:
        server: time.nist.gov
  roles:
    - role: ansible-role-shelly
      delegate_to: localhost
```

License
-------

GPLv3

Author Information
------------------

Vladimir Vasilev (@vladi-k)
