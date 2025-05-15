Script di Backup Giornaliero della Home
📄 Descrizione
Questo progetto implementa un sistema di backup automatico giornaliero per la directory /home su sistemi Linux. Lo script crea archivi compressi .tar.gz nella directory /opt/backup e mantiene una rotazione intelligente dei file, conservando solo gli ultimi 7 backup per ottimizzare lo spazio su disco.

🎯 Funzionalità principali
Backup completo: Archivia l'intera directory /home con tutti i dati degli utenti
Compressione efficiente: Utilizza il formato .tar.gz per ridurre le dimensioni dei file di backup
Rotazione automatica: Mantiene solo gli ultimi 7 backup, eliminando automaticamente i più vecchi
Timestamp integrato: Ogni file di backup include data e ora di creazione nel nome
Automazione completa: Può essere configurato per eseguirsi automaticamente tramite cron
🛠️ Installazione e configurazione
1. Preparazione dell'ambiente
Per prima cosa, creiamo la directory di destinazione per i backup:

bash
Run
sudo mkdir -p /opt/backupsudo chown $USER:$USER /opt/backup
2. Creazione dello script di backup
Crea un nuovo file chiamato backup.sh:

bash
Run
nano backup.sh
Copia e incolla il seguente codice:

bash
Run
#!/bin/bash# Cartella di backupbackup_dir="/opt/backup"# Creazione della cartella di backup se non esistemkdir -p "$backup_dir"# Nome del file di backup con data e orabackup_file="$backup_dir/backup-home-$(date +'%Y%m%d-%H%M%S').tar.gz"# Creazione del backup compresso della /hometar -czf "$backup_file" /home# Mantieni solo gli ultimi 7 backup, elimina i più vecchicd "$backup_dir"ls -tp | grep -v '/$' | tail -n +8 | xargs -I {} rm -- {}
Salva il file premendo Ctrl+O, conferma con Invio, quindi esci con Ctrl+X.

3. Impostazione dei permessi di esecuzione
Rendi lo script eseguibile:

bash
Run
chmod +x backup.sh
4. Test manuale dello script
Esegui lo script manualmente per verificare che funzioni correttamente:

bash
Run
./backup.sh
Dopo l'esecuzione, controlla che il backup sia stato creato nella directory /opt/backup:

bash
Run
ls -la /opt/backup
5. Configurazione dell'automazione con cron
Per eseguire automaticamente il backup ogni giorno alle 2:00 di notte:

bash
Run
sudo crontab -e
Aggiungi la seguente riga, sostituendo /percorso/assoluto/backup.sh con il percorso completo dello script:

cron

0 2 * * * /percorso/assoluto/backup.sh
Salva e chiudi l'editor.

🔍 Come funziona lo script
Verifica della directory di backup: Lo script controlla se la directory /opt/backup esiste e la crea se necessario.

Generazione del nome file: Crea un nome univoco per il file di backup includendo data e ora nel formato YYYYMMDD-HHMMSS.

Creazione dell'archivio: Utilizza il comando tar con le opzioni:

-c: Crea un nuovo archivio
-z: Comprime l'archivio con gzip
-f: Specifica il nome del file di output
Gestione della rotazione: Mantiene solo gli ultimi 7 backup attraverso questi passaggi:

ls -tp: Elenca i file ordinati per data, con i più recenti in cima
grep -v '/$': Esclude le directory
tail -n +8: Seleziona dal file 8 in poi (escludendo i primi 7)
xargs -I {} rm -- {}: Elimina i file selezionati
📋 Comandi utili di riferimento
Visualizzare i backup esistenti:

bash
Run
ls -lh /opt/backup
Estrarre un backup specifico:

bash
Run
tar -xzf /opt/backup/nome-del-backup.tar.gz -C /directory/destinazione
Verificare lo spazio occupato dai backup:

bash
Run
du -sh /opt/backup
Verificare i job cron configurati:

bash
Run
sudo crontab -l
⚠️ Note importanti e considerazioni
Spazio su disco: Assicurati che ci sia sufficiente spazio libero nella partizione contenente /opt/backup.

Permessi: L'utente che esegue lo script (o cron) deve avere permessi di lettura sulla directory /home e permessi di scrittura su /opt/backup.

Esclusioni: Questo script esegue il backup dell'intera directory /home. Per escludere file o directory specifici, modifica il comando tar aggiungendo l'opzione --exclude.

Sicurezza: Considera la crittografia dei backup se contengono dati sensibili.

Backup remoti: Per una maggiore sicurezza, considera di copiare i backup su un server remoto o un dispositivo esterno.

🔄 Personalizzazioni possibili
Escludere directory specifiche:

bash
Run
tar -czf "$backup_file" --exclude="/home/user/directory_da_escludere" /home
Aggiungere notifiche via email:

bash
Run
echo "Backup completato: $backup_file" | mail -s "Backup giornaliero completato" tuo@email.com
Compressione più efficiente (richiede più CPU):

bash
Run
tar -c /home | xz -9 > "$backup_file"
📜 Licenza
Questo script è fornito a scopo didattico e personale, senza alcuna garanzia. Sei libero di modificarlo e distribuirlo secondo le tue necessità.

Per ulteriori informazioni o assistenza con script di monitoraggio spazio o altre automazioni Linux, non esitare a contattarmi!
