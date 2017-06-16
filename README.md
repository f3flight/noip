# noip
no-ip.com DNS updater service for Linux. Yet another implementation, I know I know. But this one is more secure, not cleartext passwords in command line or in http (https is used).

# How to use:
1. copy files to expected destinations in /etc and /usr
2. `noip -h` - read help
3. edit (rename if you want) /usr/noip/mynoipcreds.netrc
4. edit /etc/noip/noip.conf
5. `systemctl daemon-reload`
6. `systemctl start noip`
7. `systemctl status noip` - check log, if all good then proceed
8. `systemctl enable noip` - enable to start on boot
9. be happy!

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
    public IP. Put in quotes if spaces are present. If not provided,
    default is 'curl -s http://myexternalip.com/raw'. Useful if you don't want to
    rely on http://myexternalip.com/raw to determine your public IP and have a
    different way to determine.
    Example: "ip -f inet a show br-extenral | grep -Po 'inet \K[\d.]+'"
Default check-interval is 60.

This program reads data from /etc/noip/noip.conf and updates relevant DNS records managed by no-ip.com.
Current IP address is re-checked using [ip_command] every [check-interval] seconds, upon change the DNS records are updated.
The URL https://dynupdate.no-ip.com/nic/update is used to update DNS records.
```
