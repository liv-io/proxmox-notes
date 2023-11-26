# Proxmox 8 Upgrade

## Check

```
pve7to8
```

## Resolve

```
apt purge systemd-timesyncd
apt install --no-install-recommends -y chrony
```

## VMs

``` 
for vm in $(qm list | grep 'running' | tr -s ' ' '#' | cut -d '#' -f2) ; do echo ${vm} ; done
qm list
```

## Repositories

```
sed -i 's@bullseye@bookworm@g' /etc/apt/sources.list
sed -i 's@bullseye@bookworm@g' /etc/apt/sources.list.d/proxmox.list
```

## Update

```
apt update
```

## Upgrade

```
apt dist-upgrade
```

## Reboot

```
reboot
```

## Repositories

```
sed -i 's@^deb @#deb @g' /etc/apt/sources.list.d/pve-enterprise.list
```

## Update

```
apt update
```

## ZFS

```
for pool in $(zpool list -Ho name) ; do zpool upgrade ${pool} ; done
zpool list
```

## Logs

```
grep -irE "fatal|emerg|alert|crit|err|warn|corrupt|fail" /var/log/
```
