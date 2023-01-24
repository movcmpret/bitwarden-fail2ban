# Bitwarden Fail2Ban config (Login Attempts)

This filter checks bitwardens `access.log` (nginx) for HTTP code `400` for the path `identity/connect/token`

### Docker setup with Host Reverse Proxy 

copy `filter.d/bitwarden_docker_host_reverse_proxy.conf` to `/etc/fail2ban/filter.d/bitwarden.conf` 

add this to `/etc/fail2ban/jail.local`:

	[bitwarden]
	enabled = true
	filter = bitwarden
	logpath = /path/to/your/bwdata/logs/nginx/access.log
	maxretry = 5
	bantime = 300
	port = http,https

adjust `maxretry` and `bantime`

test with `sudo fail2ban-regex /path/to/your/bwdata/logs/nginx/access.log /etc/fail2ban/filter.d/bitwarden`

If your log records only show docker internal ip addresses try to adjust the `real_ips` property in `bwdata/config.yml`:
```
# Defined as a dictionary, e.g.:
# real_ips: ['10.10.0.0/24', '172.16.0.0/16']
real_ips:
- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16
```

### Docker-only Setup

copy `filter.d/bitwarden_docker.conf` to `/etc/fail2ban/filter.d/bitwarden.conf`

add this to `/etc/fail2ban/jail.local`:

	[bitwarden]
	enabled = true
	filter = bitwarden
	logpath = /path/to/your/bwdata/logs/nginx/access.log
	maxretry = 5
	bantime = 300
	port = http,https

adjust `maxretry` and `bantime`

test with `sudo fail2ban-regex /path/to/your/bwdata/logs/nginx/access.log /etc/fail2ban/filter.d/bitwarden`
