# Ansible-Rolle: dnf_reposync

Diese rolle stellt einen reposerver zur verfügung, damit nicht jedes System seine Updates aus dem Internet ziehen muss. Das spart Zeit und Brandbreite.

## 📦 Features

- Nutzt `dnf reposync` für metadatensichere Paketspiegelung
- Führt `createrepo_c` zur Sicherstellung der Nutzbarkeit als lokales Repo aus
- Nutzt `nginx` um die Repos local über eine http Schnittstelle bereit zu stellen
- Erstellt systemd Service & Timer für tägliche Ausführung
- Zielverzeichnis: `/srv/repos/<repo-name>`

## 📁 Struktur

```text
/srv/repos/
├── baseos/
├── appstream/
├── epel/
└── ...
```

## 🧰 Voraussetzungen

- Betriebssystem: Rocky Linux 9 / RHEL 9
- Installierte Tools:
  - `dnf-utils`
  - `createrepo_c`

## 🧪 Verwendung im Playbook

```yaml
- name: Lokale Repo-Spiegelung einrichten
  hosts: yourhost
  become: true
  roles:
    - dnf_reposync
```

## 🔁 Manuelle Ausführung

```bash
sudo /usr/local/bin/sync_repos.sh
```

## 📤 Bereitstellung als lokales Repository

Du kannst `/srv/repos` z. B. via `nginx` oder `httpd` bereitstellen:

```nginx
server {
    listen 80;
    server_name repos.local;
    root /srv/repos;
    autoindex on;
}
```
