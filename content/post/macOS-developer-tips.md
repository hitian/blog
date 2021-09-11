---
title: macOS tips for developer
date: 2021-09-10T14:03:48+08:00
categories: notes
tags:
- macOS
comments: true
---

# Alias

```bash

alias show_tcp_port="lsof -nP -iTCP"
alias show_tcp_port_listen="lsof -nP -iTCP | grep LISTEN"

```

# disable .DS_Store on network storage

```bash

defaults write com.apple.desktopservices DSDontWriteNetworkStores true

```

# disable gatekepper for binary exec

e.g. perforce command `p4`

```shell

tian@mac-mini ~ % xattr -p com.apple.quarantine `which p4`
0082;5fb2322c;Safari;
tian@mac-mini ~ % xattr -d com.apple.quarantine `which p4`
xattr: [Errno 13] Permission denied: '/usr/local/bin/p4'
tian@mac-mini ~ % sudo xattr -d com.apple.quarantine `which p4`
Password:
tian@mac-mini ~ % xattr -p com.apple.quarantine `which p4`
xattr: /usr/local/bin/p4: No such xattr: com.apple.quarantine
tian@mac-mini ~ %

```

# create macOS install usb disk

```bash

sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/UDISK --applicationpath /Applications/Install\ macOS\ Big\ Sur.app

```
