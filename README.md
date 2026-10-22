# Minecraft Java server on a home machine: free quick-start (Paper, Linux)

A short guide to get a Paper server running for you and your friends. Uses free software only. Commands are for Ubuntu or Debian.

## RAM guide

| Players | RAM for the server | Notes |
|---|---|---|
| 1-5 | 3-4 GB | Any 4-core CPU from the last 10 years |
| 5-15 | 6-8 GB | Pre-generate the world; limit view distance |
| 15+ | 8-12 GB | Single-core speed matters most |

## Install

```bash
sudo apt update && sudo apt install -y openjdk-21-jre-headless tmux
sudo useradd -r -m -d /srv/mc -s /bin/bash mc
sudo -u mc mkdir -p /srv/mc/server /srv/mc/backups
# download the Paper jar for your version from papermc.io/downloads as /srv/mc/server/paper.jar
echo "eula=true" | sudo -u mc tee /srv/mc/server/eula.txt
```

Check which Java version your Minecraft version needs on the Paper download page.

## Start script (`/srv/mc/start.sh`)

```bash
#!/bin/bash
cd /srv/mc/server
exec java -Xms4G -Xmx4G -XX:+UseG1GC -XX:+ParallelRefProcEnabled \
 -XX:MaxGCPauseMillis=200 -XX:+DisableExplicitGC -XX:+AlwaysPreTouch \
 -jar paper.jar --nogui
```

Leave 1-2 GB of RAM for the operating system.

## Let friends connect safely

Use **Tailscale** instead of opening ports: install it on the server and on each friend's PC, share the server machine, and have them join with the Tailscale IP and port 25565. If you forward port 25565 on your router instead, keep the whitelist on and keep Paper updated. Never forward SSH or RCON.

## Must-have settings

`white-list=true`, `enforce-whitelist=true`, `online-mode=true`, `view-distance=8`, `simulation-distance=6`.

## Want the rest?

The full **Minecraft Home Server Runbook** adds the systemd service, backup script with retention, plugin safety, a troubleshooting table and a security checklist: https://spectre5.gumroad.com/l/nyfzyup ($6)

More guides: https://patreon.com/123black

*Not affiliated with Mojang or Microsoft.*
