# noip
noip.com DNS updater service for Linux. Can update IP **AND** confirm expiring hosts. Works over HTTPS.

# How to use:
0. Requires: jq (Shell JSON parser), curl
1. copy files to expected destinations in /etc and /usr
2. `noip -h` - read help
3. edit (rename if you want) /usr/noip/mynoipcreds.netrc
4. edit /etc/noip/noip.conf
5. `sudo systemctl daemon-reload`
6. `sudo systemctl start noip`
7. `sudo systemctl status noip` - check log, if all good then proceed
8. `sudo systemctl enable noip` - enable to start on boot
9. **Don't forget to secure your creds:** `sudo chmod 600 /etc/noip/*.netrc`
10. be happy!

# Output of `noip -h`
```
[root@host ~]$ noip -h
USAGE: noip [check-interval]
CONFIG FILE: /etc/noip/noip.conf
CONFIG FILE SYNTAX:
    <hostname> <cred-file> [ip_command]
    Tab-separated. Can contain multiple lines.
Config parameter <cred-file> should specify a path to a curl netrc file.
    Syntax is:
        machine dynupdate.no-ip.com login <login> password <password>
    See 'man curl' for details.
Config parameter [ip_command] is optional. It specifies a command
    passed to 'bash -c [ip_command] 2>&1' to determine current
    public IP. If not provided, default is 'curl -s http://myexternalip.com/raw'.
    Useful if you don't want to rely on http://myexternalip.com/raw to determine
    your public IP when you have a different way to determine.
    Example: "ip -f inet a show br-extenral | grep -Po 'inet \K[\d.]+'"
Default check-interval is 60.

This program reads data from /etc/noip/noip.conf and updates relevant DNS records managed by noip.com.
It also auto-confirms any expiring hosts via noip API.
Current IP address is re-checked using [ip_command] every [check-interval] seconds, upon change the DNS records are updated.
The URL https://dynupdate.no-ip.com/nic/update is used to update DNS records.
Expiry check is performed per .netrc file, once every 7200 seconds.
The URL https://my.noip.com/api/host/<host-id>/touch is used to auto-confirm expiring hosts.
```
