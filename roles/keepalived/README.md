# Ansible Role: Keepalived

Diese Ansible-Rolle installiert und konfiguriert **Keepalived** auf Debian- und RedHat-basierten Linux-Systemen.

## Features

- Plattformübergreifende Installation (Debian & RedHat)
- Dynamische Konfiguration über `keepalived_config`
- Template für `keepalived.conf` basierend auf Variablen
- Konfigurationstest vor Deployment
- Handler für Reload
- `become: true` für alle Tasks
- Tag: `keepalived`

## Beispielkonfiguration (`group_vars/all/keepalived_config.yml`)

```yaml
keepalived_config:
  global_defs:
    router_id: lb01
  vrrp_scripts:
    - name: chk_haproxy
      script: "/usr/bin/pgrep haproxy"
      interval: 2
      weight: -5
      fall: 2
      rise: 2
  vrrp_instances:
    - name: lb-k3s.core.lenhardt.cc
      state: BACKUP
      interface: enp6s19
      virtual_router_id: 52
      priority: 99
      advert_int: 1
      authentication:
        auth_type: PASS
        auth_pass: geheim12321451
      unicast_src_ip: 192.168.1.1
      unicast_peer:
        - 192.168.1.2
      virtual_ipaddress:
        - 10.30.30.20/32 dev enp6s18 label enp6s18:vip
      track_script:
        - chk_haproxy
    - name: lb.core.lenhardt.cc
      state: BACKUP
      interface: enp6s19
      virtual_router_id: 51
      priority: 101
      advert_int: 1
      authentication:
        auth_type: PASS
        auth_pass: geheim123
      unicast_src_ip: 192.168.1.1
      unicast_peer:
        - 192.168.1.2
      virtual_ipaddress:
        - 10.30.30.10/32 dev enp6s18 label enp6s18:vip
      track_script:
        - chk_haproxy
```

## Tags

- `keepalived` – Alle Tasks dieser Rolle

## Nutzung

```yaml
- hosts: loadbalancers
  roles:
    - role: keepalived
```
