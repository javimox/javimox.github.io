---
title:  "mox helm repository goes live"
toc: true
categories: 
  - Linux
tags:
  - systemd
  - buster
  - rc.local
  - bluethoot
last_modified_at: 2019-05-28T18:22:36+02:00
---

# rc.local deprecated

The _old_ friend `rc.local` has been in Debian 10 deprecated. Using Debian 10 Buster we can find a new way to keep using a rc.local startup script, though.

In this example we want to disable bluetooth, as every time the system boots up is bluetooth enabled.

## systemd Service

We create a systemd unit, that will launch a _oneshot_ script located in `/usr/local/bin/`

File: `/etc/systemd/system/local-starttasks.service`

```bash
[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/local/bin/rc.local

[Install]
WantedBy=multi-user.target

[Unit]
Wants=bluetooth.service
After=bluetooth.service
```

Then we can enable it running:

```bash
systemctl enable local-starttasks.service
```
 
To finish the example, this is the content of our new `rc.local` script:

File `/usr/local/bin/rc.local`

```bash 
#!/bin/bash
echo disable > /proc/acpi/ibm/bluetooth
```
