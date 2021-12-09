---
title: ttyd 配合 tmux
date: 2021-09-27 15:20:46
tags: 
  - linux 
  - sudo
categories:
  - linux
---

```
[Unit]
Description=TTYD
After=syslog.target
After=network.target

[Service]
ExecStart=/opt/ttyd tmux a -t ttyd
Type=simple
Restart=always
User=someuser
Group=someuser
ExecStartPre=-tmux has-session -t "ttyd" || tmux new-session -d -s "ttyd" "tmux set -g status off && /bin/bash"

[Install]
WantedBy=multi-user.target
```
