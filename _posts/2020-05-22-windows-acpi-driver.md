---
layout: post
title: Fixing missing driver in Windows 10 virt-manager guest
---

When using Windows with virt-manager, libvirt, qemu, or any kvm based hypervisor I always install the spice vdagent. This installs most of the required drivers for the attached devices but I always have one device left after installing [spice guest tools](https://www.spice-space.org/download/windows/spice-guest-tools/spice-guest-tools-latest.exe) and it's a generic ACPI device.

Device Instance path:
```
ACPI\APP0005\3&2411E6F3&1
```

Hardware Ids:
```
ACPI\VEN_APP&DEV_0005
ACPI\APP0005
*APP0005
```

To fix this, go to

```
Start > Device Manager > Right click device > Update Drivers > Browse my computer for driver software > Let me pick from a list of available drivers on my computer > System devices > Next > (Standard system devices) > ACPI Processor Container Device > Next > Yes > Close
``` 

Thanks to [AntaresUK](https://forums.unraid.net/profile/85579-antaresuk/) for posting the [solution](https://forums.unraid.net/topic/87453-solved-win-10-unknown-device-error-acpiven_appdev0005/).