# AB-WebUI
This role installs following packages and configures them
- ntp
- monit
- nginx 
- kibana

## Role parameters
### NTP
ntp is configured using the crontab which updates system time on midnight.

There is ```time.google.com``` time server is set by default you can give another time server by parameter ```ntp_server```:

```yml
- role: ab-webui
  ntp_server: ntp.server.url
```

### ini file path
We can give id file and path and it's download path to local machine

```vmid_path``` path may be specified during vagrant image intalization

```vmid_fetch_dir```'s value is ```/tmp``` directory by default

```yml
- role: ab-webui
  vmid_path: /path/to/id/file # this maybe configured during vagrant initalization
  vmid_fetch_dir: /path/to/fetch/file.ini 
```

### setting elasticsearch server

```yml
- role: ab-webui
  elastic_address: 192.168.1.102:9200
```

### monit
monit is configured so, if there is any problem with kibana, then restarts the kibana servers

```yml
check process kibana with pidfile /var/run/kibana/kibana.pid
  start program = "/etc/init.d/kibana start"
  stop program  = "/etc/init.d/kibana stop"
  if failed host 127.0.0.1 port 5601 protocol http
    and request "/app/home"
    with timeout 10 seconds
  then restart
  if 3 restarts within 5 cycles then timeout
```