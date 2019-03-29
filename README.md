# ManageIQ Deployment Orchestration with Ansible

This project consists of code which installs a 5 node Manage IQ cluster in 11 steps.

## Requisites:

 1. You have 5 Manage IQ virtual appliances deployed and running in
    their default OR mostly default state ( eg. If you had to configure
    networking because DHCP or Cloud-init is not configured this is fine, but nothing else).
 2. SSH Keys are installed (via cloud init / or manual copy)
 3. The code is meant to be run in the order it is numerically prefixed

## Notes

 - We use YAML based inventory instead of the more common INI style as several variables are ugly to express in single line JSON stream.
 - Database nodes **require** an unformatted secondary disk to the appliance image and that disk needs to be defined in dbDisk.  See official Red Hat Cloudforms deployment documents describe appropriate sizing for the DB disk based.
 - 07-evm-config.yml requires that you correctly label your non database MIQ cluster members by short name in the **serverRoles:** dictionary and tag them for what their purpose will be.

 For example:
 
    serverRoles:
      portal:
        "{{ uiNode }}"
      worker01:
        "{{ workerNode }}"
      worker02:
        "{{ workerNode }}"
      worker03:
        "{{ workerNode }}"

 - Any custom AD group mappings can be added by editing the **customGroups** dictionary also defined in the inventory file.
 - Tenants are defined in the array **exampleTenants** and are deployed as MIQ projects (tenants that can have no sub tenants).
