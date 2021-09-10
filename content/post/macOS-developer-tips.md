---
categories: notes
date: "2021-09-10T13:43:00Z"
tags:
- macOS
title: macOS tips for developer
---

# Alias

```bash
alias k="kubectl"
alias nerd="lima nerdctl"
alias show_tcp_port="lsof -nP -iTCP"
alias show_tcp_port_listen="lsof -nP -iTCP | grep LISTEN"
```

# disable .DS_Store on network storage

```

defaults write com.apple.desktopservices DSDontWriteNetworkStores true

```