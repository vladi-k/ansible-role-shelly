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
* `shelly_device_profile` - device profile name.
* `shelly_rpc_device_profile` - dict to call `Shelly.SetProfile` method.
* `shelly_wait_for` - seconds to wait for shelly to become available again.
* `shelly_rpc_data` - list to pass to JSON-RPC. Check [official documentation](https://shelly-api-docs.shelly.cloud/gen2/) for available settings. Example:
```yaml
shelly_rpc_data:
  - id: 1
    method: Sys.SetConfig
    params:
      config:
        device:
          name: "{{ inventory_hostname }}"
          profile: cover
        location:
          tz: Europe/Prague
        sntp:
          server: time.nist.gov
  - id: 1
    method: Cover.SetConfig
    params:
      id: 0
      config:
        power_limit: 120
```
* `shelly_rpc_retries` - number of retries when calling JSON-RPC.
* `shelly_rpc_delay` - delay between retries when calling JSON-RPC.
* `shelly_rpc_sys_get_status` - dict to call `Sys.GetStatus` method.
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
    shelly_device_profile: cover
    shelly_password: verysecure
    shelly_rpc_data:
      - id: 1
        method: Sys.SetConfig
        params:
          config:
            device:
              name: "{{ inventory_hostname }}"
              profile: cover
            location:
              tz: Europe/Prague
            sntp:
              server: time.nist.gov
      - id: 1
        method: Cover.SetConfig
        params:
          id: 0
          config:
            power_limit: 120
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
