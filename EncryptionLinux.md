# Cheat Sheet de l'encryption avec LUKS

## Identification de la partition cible  

```bash
lsblk
```  
Cette commande affichera tous les dispositifs de stockage et partitions attachés. Recherchez la partition que vous souhaitez crypter (par exemple, /`dev/sdb1`).  

## Initialisation LUKS sur la partition  

```bash
sudo cryptsetup luksFormat /dev/sdb1
```  
Lors de cette étape, il va falloir définir une phrase secrète. La phrase que vous définisser ici sera nécessaire pour déverrouiller la partition plus tard.  ⚠️ cela va écraser toutes les données sur le disque/partition

## Ouvrir la partition cryptée  

```bash
sudo cryptsetup luksOpen /dev/sdb1 partition_cryptée
```  
Cela crée un nouveau mappage de dispositif, généralement situé sous /dev/mapper/partition_cryptée. Vous pouvez nommer le mappage comme vous le souhaitez.  

## Créer un système de fichiers sur la partition chiffrée  

```bash
sudo mkfs.ext4 /dev/mapper/partition_cryptée
```  
Vous avez maintenant une partition cryptée avec un système de fichiers prêt à l’emploi. Le système de fichier utilise `ext4`  

## Monter la partition cryptée  
```bash
sudo mount /dev/mapper/partition_cryptée /mnt
```  
Vous pouvez désormais stocker des données dans /mnt, et elles seront cryptées automatiquement.  

----
## Monter une partition cryptée au démarrage  

Si vous souhaitez que la partition cryptée soit automatiquement disponible au démarrage, vous devrez l’ajouter aux fichiers /etc/crypttab et /etc/fstab de votre système.
1. Modifier `/etc/crypttab` :  
Ajoutez la ligne suivante, en remplaçant partition_cryptée et /dev/sdb1 par les noms appropriés à votre configuration : 
```bash
partition_cryptée /dev/sdb1 none luks
```  

2. Modifier `/etc/fstab` :  
Ajoutez la ligne suivante pour monter la partition cryptée au démarrage :  
```bash
/dev/mapper/partition_cryptée /mnt ext4 defaults 0 2
```  
Avec ces entrées en place, votre partition cryptée sera déverrouillée et montée automatiquement au démarrage du système. Toutefois, vous serez invité à saisir la phrase secrète LUKS lors du démarrage.


[tuto crypter avec LUKS](https://www.webhi.com/how-to/fr/systemes-de-fichiers-cryptes-avec-luks-sur-linux/)  


---