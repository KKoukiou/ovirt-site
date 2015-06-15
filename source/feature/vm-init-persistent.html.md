---
title: vm-init-persistent
authors: shaharh
wiki_title: Features/vm-init-persistent
wiki_revision_count: 6
wiki_last_updated: 2014-02-12
---

# vm-init-persistent

## VM-Init Persistent

### Summary

This Feature will allow persistent of Windows Sysprep and Cloud-Init data to the Database. By persisting the data Admin can create a template with VM-Init data that which will enable initialize VMs with relevant Data.

### Owner

*   Name: Shahar Havivi
*   Email: shaharh@redhatdotcom

### Detailed Description

Currently we are persisting the Timezone and Domain for Sysprep Windows based machines and Cloud-init data via Run-Once dialog. We want to be able to add more granularity to Sysprep such as saving a Windows serial key and persisting the current Cloud-init data that we have via the Run-Once dialog. The VM-Init section will have its own tab in Add/Edit VM and will stay the same for the Run-Once dialog.

By adding a new table for initialization purpose of VMs the Domain field will move from vm_static table (VmBase business object) to the vm_init table (VmInit business object). We will have two forms of time-zone, The one that we currently have in vm_static will move to the System tab section in Add/Edit VM, this will represent the time-zone of the BIOS, the one that is represented in the qemu command line, The new time-zone that vm_init table have will represent the VM operating system time-zone.

### VM-Init Table

         vm_id
         host_name
         domain
         authorized_keys
         regenerate_keys
         time_zone
         dns_servers
         dns_search_domains
         networks
         password
         winkey
         custom_script

### Custom Script

Custom Script enable Cloud-Init the ability to add custom YAML sections (which is concatenated to the YAML generated by oVirt) such as adding file:

      write_files:
      -   content: |
              # THIS IS MY TEXT FILE
              Some Content for my file
          path: /tmp/myfile
          permissions: '0644'

More inforamation please refer to [Cloud-Init](http://cloudinit.readthedocs.org/en/latest/topics/examples.html#yaml-examples) Documentations.

**Please note that an error in custom script will cause the Cloud-Init loading failure.**

### UX Design

[UX Design page](UX/cloud_init)

![](Cloud_init.jpg "Cloud_init.jpg")

![](Cloud_init_collapsed.png "Cloud_init_collapsed.png")