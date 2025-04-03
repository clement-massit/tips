# Cheat Sheet des Commandes Rsync

## 1. Commande de Base
```bash
rsync -avz source/ destination/
```
- `-a` : Mode archive (préserve les permissions, timestamps, etc.).
- `-v` : Mode verbeux (affiche les fichiers transférés).
- `-z` : Compression des fichiers pendant le transfert.

## 2. Synchronisation avec un Serveur Distant
```bash
rsync -avz /local/path/ user@remote_host:/remote/path/
```
- Synchronisation de fichiers du local vers un serveur distant via SSH.

```bash
rsync -avz user@remote_host:/remote/path/ /local/path/
```
- Synchronisation du serveur distant vers le local.

## 3. Suppression des Fichiers Supprimés sur la Source
```bash
rsync -avz --delete source/ destination/
```
- Supprime les fichiers qui ne sont plus présents dans la source.

## 4. Simulation (Dry Run)
```bash
rsync -avzn source/ destination/
```
- Affiche ce qui serait synchronisé sans réellement effectuer le transfert.  
 ou bien : 
```bash
rsync -avz --dry-run source/ destination/
```

## 5. Exclure des Fichiers ou Répertoires
```bash
rsync -avz --exclude='*.log' --exclude='cache/' source/ destination/
```
- Exclut les fichiers `.log` et le répertoire `cache/`.

## 6. Utilisation avec SSH
```bash
rsync -avz -e "ssh -p 2222" source/ user@remote_host:/remote/path/
```
- Spécifie un port SSH différent (`2222`).

## 7. Afficher la Progression du Transfert
```bash
rsync -avz --progress source/ destination/
```
- Affiche la progression détaillée des fichiers en cours de transfert.

## 8. Limiter la Bande Passante
```bash
rsync -avz --bwlimit=1000 source/ destination/
```
- Limite la vitesse de transfert à 1000 KB/s.

## 9. Synchronisation en Mode Backup avec Horodatage
```bash
rsync -avz --backup --backup-dir=/backup/$(date +%Y-%m-%d) source/ destination/
```
- Sauvegarde les fichiers modifiés dans un dossier horodaté avant écrasement.

## 10. Planifier une Synchronisation avec Cron
Ouvrir l'éditeur de cron :
```bash
crontab -e
```
Ajouter une tâche planifiée (exécution quotidienne à 2h du matin) :
```bash
0 2 * * * rsync -avz --delete /var/www/ user@remote_host:/var/www/ >> /var/log/rsync.log 2>&1
```

---  

Sur le Serveur 1 (prod) : 
    pouvoir se connecter en ssh sur le serveur 2



Sur le serveur 2 : 
    Autoriser la clé ssh du serveur 1 

