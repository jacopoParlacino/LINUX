# Script di Backup Giornaliero della Home

## 📄 Descrizione

Questo progetto contiene uno script per eseguire un **backup giornaliero** della directory `/home` su Linux. Il backup viene salvato in una cartella `/opt/backup` e viene mantenuta una rotazione di massimo 7 file di backup.

---

## 🛠️ Passaggi effettuati

### 1. Creazione della cartella di destinazione

Per prima cosa abbiamo creato la cartella `/opt/backup` dove verranno salvati i backup:

```bash
sudo mkdir -p /opt/backup
sudo chown $USER:$USER /opt/backup

#!/bin/bash

# Cartella di backup
backup_dir="/opt/backup"

# Creazione della cartella di backup se non esiste
mkdir -p "$backup_dir"

# Nome del file di backup con data e ora
backup_file="$backup_dir/backup-home-$(date +'%Y%m%d-%H%M%S').tar.gz"

# Creazione del backup compresso della /home
tar -czf "$backup_file" /home

# Mantieni solo gli ultimi 7 backup, elimina i più vecchi
cd "$backup_dir"
ls -tp | grep -v '/$' | tail -n +8 | xargs -I {} rm -- {}

chmod +x backup.sh

./backup.sh
