---
layout: post
title: Setting up CPU governor for battery/ac switch using systemd
---

Found a great walkthrough for using systemd to switch CPU governors:

[https://chrisdown.name/2017/10/29/adding-power-related-targets-to-systemd.html](https://chrisdown.name/2017/10/29/adding-power-related-targets-to-systemd.html)

Make udev rule to trigger systemd target:

```
/etc/udev/rules.d/99-powertargets.rules

SUBSYSTEM=="power_supply", KERNEL=="AC", ATTR{online}=="0", RUN+="/usr/bin/systemctl start battery.target"
SUBSYSTEM=="power_supply", KERNEL=="AC", ATTR{online}=="1", RUN+="/usr/bin/systemctl start ac.target"
```


Make systemd targets for ac/battery:

```
/etc/systemd/system/ac.target

[Unit]
Description=On AC Power
DefaultDependencies=no
StopWhenUnneeded=yes
```

```
/etc/systemd/system/battery.target

[Unit]
Description=On battery power
DefaultDependencies=no
StopWhenUnneeded=yes
```

Make services for ac/battery:

```
/etc/systemd/system/ac-governor.service

[Unit]
Description=Sets governor on ac power

[Service]
Type=oneshot
ExecStart=cpupower frequency-set --governor performance

[Install]
WantedBy=ac.target
```

```
/etc/systemd/system/battery-governor.service

[Unit]
Description=Sets governor on battery power

[Service]
Type=oneshot
ExecStart=cpupower frequency-set --governor powersave

[Install]
WantedBy=battery.target
```

Enable the services to install on the targets

```
sudo systemctl enable ac-governor.service
sudo systemctl enable battery-governor.service
```



