ansible-role-shelly
====

Configure Shelly devices.

Note that even in [check mode](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_checkmode.html) the role will POST your configuration to Shelly Gen 2 (Shelly-NG) based on `shelly_rpc_data` variable.

Tested on:
* [Shelly Dimmer 2](https://kb.shelly.cloud/knowledge-base/shelly-dimmer-2)
* [Shelly Plus 1PM](https://kb.shelly.cloud/knowledge-base/shelly-plus-1pm)
* [Shelly Plus 2PM](https://kb.shelly.cloud/knowledge-base/shelly-plus-2pm)
* [Shelly Pro 3](https://kb.shelly.cloud/knowledge-base/shelly-pro-3-v1)
* [Shelly Plus 1PM Mini](https://kb.shelly.cloud/knowledge-base/shelly-plus-1pm-mini)
* [Shelly Plus i4](https://kb.shelly.cloud/knowledge-base/shelly-plus-i4)


Requirements
------------

* curl

Role Variables
--------------

* `shelly_gen` - set shelly device generation. Can be `1` or `2`.
* `shelly_gen1_config` list of Gen 1 configuration, example:
```yaml
shelly_gen1_config:
  - path: /settings
    params:
      name: "{{ inventory_hostname }}"
      sntp_server: time.nist.gov
      transition: 1000
      min_brightness: 12
```
* `shelly_username` - Device username.
* `shelly_password` - Device password.
* `shelly_curl_auth` - Authentication part of `curl`.
* `shelly_rpc_uri` - RPC URI.
* `shelly_rpc_sys_getconfig` - dict to call `Sys.GetConfig` method.
* `shelly_profile` - device profile name.
* `shelly_rpc_shelly_setprofile` - dict to call `Shelly.SetProfile` method.
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
* `shelly_rpc_delay` - delay in seconds between retries when calling JSON-RPC.
* `shelly_rpc_sys_getstatus` - dict to call `Sys.GetStatus` method.
* `shelly_rpc_shelly_reboot` - dict to call `Shelly.Reboot` method.
* `shelly_restart` - boolean to reboot Shelly if required.
* `shelly_cover_calibrate` - boolean to calibrate shelly cover.
* `shelly_rpc_cover_getstatus` - dict to call `Cover.GetStatus` method.
* `shelly_rpc_cover_calibrate` - dict to call `Cover.Calibrate` method.
* `shelly_cover_calibrate_retries` - number of retries when waiting for cover calibrate.
* `shelly_cover_calibrate_delay` - delay in seconds between retries when waiting for cover calibrate.
* `shelly_update` - boolean to update Shelly firmware.
* `shelly_rpc_shelly_checkforupdate` - dict to call `Shelly.CheckForUpdate`.
* `shelly_rpc_shelly_update` - dict to call `Shelly.Update`.

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
