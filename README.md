# Install warp-plus on OpenWrt
[فارسی](https://github.com/Ramtiiin/iran-ip/blob/main/README.fa.md)

⚠️ This configuration has been tested on a [Google WiFi Router](https://support.google.com/googlenest/answer/7168315?hl=en) running OpenWrt version OpenWrt 23.05.2 r23630-842932a63d stable. Ensure that your router has **at least 128MB of storage** before executing these commands. ⚠️

## 1. Initial Settings

### Install PassWall2 on OpenWrt

Read more about [Install PassWall2 on OpenWrt router](https://github.com/Ramtiiin/install-passwall2-openwrt).

## 2. Install warp-plus

### SSH into the Router

1. Open your terminal (or SSH client) and connect to the router:
   ```sh
   ssh root@192.168.1.1

2. Update the package list:
   ```sh
   opkg update

4. Install OpenSSH SFTP Server:
   ```sh
   opkg install openssh-sftp-server
   
5. [Download WARP PLUS based on your Router's CPU Architecture](https://github.com/bepass-org/warp-plus/releases)
   
   Extract the file and RENAME the binary file to > warp

7. Transfer the file to the Router > /usr/bin (with tools like WinSCP, VSCode, ...)

8. Make it executable in OpenWrt:
   ```sh
   chmod +x /usr/bin/warp
9. Create System Service file for WARP:
   ```sh
   vi /etc/init.d/warp
   
  Switch to Insert mode (Press i) & Paste this text block:
  ```sh
   #!/bin/sh /etc/rc.common

START=91

USE_PROCD=1

PROG=/usr/bin/warp

start_service() {
  args=""
  args="$args -b 127.0.0.1:9090 --scan"
  procd_open_instance
  procd_set_param command $PROG $args
  procd_set_param stdout 1
  procd_set_param stderr 1
  procd_set_param respawn
  procd_close_instance
}
 ```
Save & exit the editor (Esc > :x)

10. Change the permissions of the Service file:
    ```sh
     chmod 755 /etc/init.d/warp

11. Enable & Start service and check the status:
    ```sh
     service warp enable
     service warp start
     service warp status
    ```
12. Go to Luci-app (OpenWrt web) > Passwall2 and create a sockes config to:
    
    127.0.0.1:9090

    and Enable Main switch & Socks Config
