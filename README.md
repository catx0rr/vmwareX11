# vmwareX11

Configure and Autostart VM.

```
cp vmautostart.desktop ~/.config/autostart
```

set static IP
```
nmcli con mod <interface> connection.autoconnect yes \
ipv4.addresses X.X.X.X/X \
ipv4.gateway X.X.X.X \
ipv4.dns 8.8.8.8,8.8.4.4 \
ipv4.method manual
nmcli con down <interface> ; nmcli con up <interface>
```

Setup X11 forwarding

On CentOS/Rhel/Fedora:
```
sudo dnf install -y xorg-x11-xauth
```

On Debian/Ubuntu:
```
sudo apt install -y xauth
```

Configure SSHD on (VMs):
```
echo "X11Forwarding yes" | sudo tee -a /etc/ssh/sshd_config
systemctl restart sshd
cat .Xauthority
```

Spawn desired GUI on your host:
```
ssh -X user@ipaddr
```

## Done! Run any gui apps on your linux host from your vm.
