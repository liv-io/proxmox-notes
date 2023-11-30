# Proxmox 8 Upgrade

## Run Upgrade Checker

```
pve7to8
```

## Resolve Warnings

```
apt purge systemd-timesyncd
apt install --no-install-recommends -y chrony
```

## Shutdown Virtual Machines

``` 
for vm in $(qm list | grep 'running' | tr -s ' ' '#' | cut -d '#' -f2) ; do qm shutdown ${vm} ; done
qm list
```

## Update APT Repositories

```
sed -i 's@bullseye@bookworm@g' /etc/apt/sources.list
sed -i 's@bullseye@bookworm@g' /etc/apt/sources.list.d/proxmox.list
```

## Update APT Cache

```
apt update
```

## Run APT Upgrade

```
apt dist-upgrade
```

## Reboot System

```
reboot
```

## Disable Repository

```
sed -i 's@^deb @#deb @g' /etc/apt/sources.list.d/pve-enterprise.list
```

## Update APT Cache

```
apt update
```

## Upgrade ZFS Pools

```
for pool in $(zpool list -Ho name) ; do zpool upgrade ${pool} ; done
zpool list
```

## Inspect Logs

```
grep -irE "fatal|emerg|alert|crit|err|warn|corrupt|fail" /var/log/
```
