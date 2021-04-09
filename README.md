# vmwareX11

### Configure and Autostart VM.

On Windows Desktop(Your Host):
```
- Instructions here on how to install X Windows Server on Windows:

https://linuxhint.com/linux_graphical_windows_x11_forwarding/

- Create a startup script to execute the command and run the 
  desired vm on background on boot.

C:\Program Files(x86)\VMware\VMware Workstation\vmrun.exe -T ws start C:\path\to\vmguest\file.vmx nogui

- Install putty or mobaxterm and setup X11 forwarding and SSH in.

```
Windows 10 and WSL, XLaunch
```
- Install XLaunch X11 Server

https://linuxhint.com/linux_graphical_windows_x11_forwarding/

- Create a startup script to execute using powershell to run the desired vm on background on boot

Invoke-Expression -Command "& 'C:\Program Files (x86)\VMware\VMware Workstation\vmrun.exe' -T ws start C:\path\to\vmguest\file.vmx nogui"

- Ensure XLaunch Program runs on start for efficiency
- On WSL, create Display on your .bashrc profile

echo "export DISPLAY=localhost:0" >> ~/.bashrc

RUN: ssh -X user@ipaddr 
```

On Linux Desktop(Your Host):
```
cp vmautostart.desktop ~/.config/autostart
```

### Set static IP

On RPM distros:
```
nmcli con mod <interface> connection.autoconnect yes \
ipv4.addresses X.X.X.X/X \
ipv4.gateway X.X.X.X \
ipv4.dns 8.8.8.8,8.8.4.4 \
ipv4.method manual
nmcli con down <interface> ; nmcli con up <interface>
systemctl restart NetworkManager
```

On Debian distros:
```
# cat << EOF >> /etc/network/interfaces

auto <interface>
iface <interface> inet static
address X.X.X.X
netmask X.X.X.X
network X.X.X.0
broadcast X.X.X.255
gateway X.X.X.X
EOF
```
```
# echo "nameservers 8.8.8.8
nameservers 8.8.4.4" > /etc/resolv.conf

# /etc/init.d/networking restart
```

### Setup X11 forwarding

On RPM distros:
```
sudo dnf install -y xorg-x11-xauth
```

On Debian distros:
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

## Done! Run any gui apps on your host from your VM.
