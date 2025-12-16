# Còpies de seguretat amb Duplicity (Linux)

En aquesta guia es mostra pas a pas com configurar còpies de seguretat a un servidor Linux utilitzant **Duplicity** i automatitzant-les amb scripts i **cron**, seguint l'esquema de còpia completa setmanal i còpies incrementals diàries.

> **Abans de començar:** S'ha creat una màquina virtual amb Zorin OS (basat en Ubuntu) i s'ha afegit un segon disc dur virtual de 10 GB, que servirà com a unitat de backup externa.

![Creació del disc secundari](/tasca02/img_t02_lin/captura1.png)

---

## 1. Inicialització i formatació en XFS

### 1.1 Identificació del disc secundari

Abans de començar, cal veure quins discs té el sistema i on està el disc secundari que farem servir per al backup.  
La comanda `lsblk` llista tots els dispositius de bloc, la seva mida, tipus i punt de muntatge.

![Sortida de lsblk mostrant els discos sda i sdb](/tasca02/img_t02_lin/captura2.png)

Es pot veure que tenim:
- `sda`: disc principal amb les particions del sistema.
- `sdb`: disc secundari de 10 GB, encara sense formatar ni muntar.

### 1.2 Particionament del disc secundari amb GPT

Utilitzarem `parted` per crear una etiqueta de disc **GPT** i una partició que ocupi tot l'espai disponible.

![Creació d'una partició GPT amb parted](/tasca02/img_t02_lin/captura3.png)

Dins de parted executem:
- `mklabel gpt` → Crea una taula de particions GPT (es perd tot el contingut anterior).
- `mkpart primary xfs 1MiB 100%` → Crea una partició primària amb sistema de fitxers XFS des de l'inici fins al final del disc.
- `quit` → Surt de parted.

### 1.3 Verificació de la partició creada

Un cop creada la partició, tornem a executar `lsblk -f` per veure els sistemes de fitxers detectats.

![Sortida de lsblk -f mostrant la nova partició sdb1 sense formatar](/tasca02/img_t02_lin/captura4.png)

Es veu que `sdb1` ja existeix però encara no té cap sistema de fitxers (`FSTYPE` buit).

### 1.4 Instal·lació de les eines XFS

Per poder formatar la partició amb XFS, cal instal·lar el paquet `xfsprogs`.

![Instal·lació del paquet xfsprogs amb apt](/tasca02/img_t02_lin/captura5.png)

S'executa:
```bash
sudo apt install xfsprogs
```

### 1.5 Formatació de la partició amb XFS

Ara formatem la partició `sdb1` amb el sistema de fitxers XFS.

![Formatació de /dev/sdb1 amb mkfs.xfs](/tasca02/img_t02_lin/captura6.png)

La comanda `sudo mkfs.xfs -f /dev/sdb1` crea el sistema de fitxers XFS forçant l'escriptura encara que el dispositiu ja tingui alguna estructura.

### 1.6 Muntatge manual a /media/backup

Abans de poder usar el disc, cal crear un punt de muntatge i muntar-hi la partició.

![Creació de /media/backup i muntatge de /dev/sdb1](/tasca02/img_t02_lin/captura7.png)

S'executen:
```bash
sudo mkdir -p /media/backup
sudo mount /dev/sdb1 /media/backup
```
Així la unitat de backup queda accessible a `/media/backup`.

---

## 2. Instal·lació de Duplicity

Abans d'instal·lar Duplicity, convé actualitzar la llista de paquets.

![Actualització dels repositoris amb apt update](/tasca02/img_t02_lin/captura8.png)

Després instal·lem Duplicity, l'eina que farem servir per fer les còpies de seguretat encriptades.

![Instal·lació de duplicity](/tasca02/img_t02_lin/captura9.png)

Si ja està instal·lat, el sistema ho indica i no cal fer res més.

---

## 3. Creació d'usuaris i arxius de prova

### 3.1 Creació d'usuaris

Per simular un entorn real, creem dos usuaris addicionals.

![Creació de l'usuari tecnici amb adduser](/tasca02/img_t02_lin/captura10.png)

![Creació de l'usuari tecnico amb adduser](/tasca02/img_t02_lin/captura11.png)

Es pot veure que el sistema exigeix contrasenyes de com a mínim 8 caràcters.

### 3.2 Creació d'arxius de 10 MB

A continuació, dins del directori personal de l'usuari actual, generem 4 arxius de 10 MB cadascun amb `fallocate`.

![Creació de 4 arxius de 10 MB amb fallocate](/tasca02/img_t02_lin/captura12.png)

Després comprovem que s'han creat correctament amb `ls -lh`.

![Llistat dels arxius creats, tots de 10 MB](/tasca02/img_t02_lin/captura13.png)

---

## 4. Primera còpia de seguretat de /home

Per evitar haver d'introduir la contrasenya de xifratge cada vegada, definim una variable d'entorn `PASSPHRASE`.

![Exportació de la variable PASSPHRASE](/tasca02/img_t02_lin/captura14.png)

Això permetrà que Duplicity l'utilitzi automàticament durant l'execució dels backups.

Ara fem la primera còpia completa de tot el directori `/home` a la unitat de backup.

![Execució del full backup amb duplicity](/tasca02/img_t02_lin/captura15.png)

La comanda:
```bash
sudo duplicity full /home file:///media/backup/home_backup
```
xifra i copia tots els fitxers de `/home` al directori `home_backup` dins del disc muntat.  
La sortida mostra estadístiques: 4732 fitxers, 249 MB, temps d'execució 6,41 s.

---

## 5. Prova de restauració

### 5.1 Esborrat d'arxius

Simulem una pèrdua de dades esborrant un dels arxius creats anteriorment.

![Esborrat de l'arxiu arxiux.dat](/tasca02/img_t02_lin/captura16.png)

### 5.2 Restauració dels arxius

Ara restaurem els fitxers des de la còpia de seguretat a una carpeta temporal per comprovar que la recuperació funciona.

![Restauració del backup a /tmp/restore_test](/tasca02/img_t02_lin/captura17.png)

La comanda:
```bash
sudo duplicity restore file:///media/backup/home_backup /tmp/restore_test
```
demana la passphrase (que ja tenim en variable d'entorn) i recupera tot el contingut de `/home` a la ruta indicada.

Comprovem que a `/tmp/restore_test` hi són les carpetes dels usuaris, inclosa la de l'usuari actual amb els arxius que havíem esborrat.

![Llistat de /tmp/restore_test mostrant les carpetes d'usuari](/tasca02/img_t02_lin/captura18.png)

---

## 6. Còpia incremental

### 6.1 Creació d'un nou arxiu de 4 MB

Afegim un arxiu de 4 MB a l'home de l'usuari.

![Creació d'un arxiu de 4 MB](/tasca02/img_t02_lin/captura19.png)

### 6.2 Execució de còpia incremental

Ara fem una **còpia incremental**: només es copien els canvis des de l'últim backup.

![Execució d'un backup incremental](/tasca02/img_t02_lin/captura20.png)

La sortida mostra que s'han afegit 23 fitxers nous (21,5 MB) i canviat 33, amb un temps d'execució molt més curt (0,86 s).  
Això demostra la eficiència dels backups incrementals.

### 6.3 Desmuntatge de la unitat de backup

Abans d'automatitzar, és útil comprovar l'estat de les còpies que ja hem fet manualment.  
La comanda `duplicity collection-status` mostra la cadena de backups existent.

![Verificació de l'estat de la col·lecció de backups](/tasca02/img_t02_lin/captura21.png)

Es pot veure que hi ha una cadena principal amb:
- 1 backup complet (Full) del 16/12 a les 16:31
- 2 backups incrementals posteriors

Això confirma que tot està funcionant correctament.

Com a mesura de seguretat, desmunta la unitat de backup després de les proves manuals.

![Desmuntatge de /media/backup](/tasca02/img_t02_lin/captura22.png)

La unitat ja no estarà accessible fins que es torni a muntar, protegint així les dades de backup d'accidents o atacs.

---

## 7. Creació del script fullbackup.sh

Ara creem el primer script d'automatització per als backups complets.  
El script es guardarà a `/usr/local/bin/fullbackup.sh`.

![Creació del script fullbackup.sh amb nano](/tasca02/img_t02_lin/captura23.png)

El contingut del script és:

```bash
export PASSPHRASE=contrasenyal123

mount /dev/sdb1 /media/backup

duplicity full /home file:///media/backup/home_backup
umount /media/backup

unset PASSPHRASE
```

![Contingut del script fullbackup.sh](/tasca02/img_t02_lin/captura24.png)

El script:
1. Defineix la variable `PASSPHRASE` per al xifratge
2. Munta la unitat de backup
3. Executa el backup complet
4. Desmunta la unitat
5. Esborra la variable d'entorn per seguretat

Per poder executar el script, cal donar-li permisos d'execució.

![Assignació de permisos d'execució a fullbackup.sh](/tasca02/img_t02_lin/captura25.png)

La comanda `sudo chmod +x /usr/local/bin/fullbackup.sh` permet que el script es pugui executar com a programa.

---

## 8. Programació al cron com a root dels diumenges a les 23:00

Ara programem l'execució automàtica dels scripts mitjançant el programador de tasques **cron** de l'usuari root.

![Accés al crontab de root](/tasca02/img_t02_lin/captura26.png)

S'executa `sudo crontab -e` per editar les tasques programades del superusuari.

Dins de l'editor, afegim la línia per al backup complet:

![Primera versió del crontab amb la tasca de backup complet](/tasca02/img_t02_lin/captura27.png)

La configuració per al backup complet és:
```bash
0 23 * * 0 /usr/local/bin/fullbackup.sh
```
Això significa: **Diumenges (0) a les 23:00 → backup complet**.

---

## 9. Creació del script incrementalbackup.sh

De manera similar, creem el script per als backups incrementals.

![Creació del script incrementalbackup.sh amb nano](/tasca02/img_t02_lin/captura28.png)

El contingut és quasi idèntic, però amb la comanda `duplicity incremental`:

```bash
export PASSPHRASE=contrasenyal123

mount /dev/sdb1 /media/backup

duplicity incremental /home file:///media/backup/home_backup
umount /media/backup

unset PASSPHRASE
```

![Contingut del script incrementalbackup.sh](/tasca02/img_t02_lin/captura29.png)

De la mateixa manera, donem permisos d'execució al segon script.

![Assignació de permisos d'execució a incrementalbackup.sh](/tasca02/img_t02_lin/captura30.png)

---

## 10. Programació al cron de dilluns a dissabte a les 23:00

Completem la configuració del crontab afegint la línia per als backups incrementals:

![Versió final del crontab amb totes les tasques](/tasca02/img_t02_lin/captura31.png)

La configuració final és:
```bash
0 23 * * 0 /usr/local/bin/fullbackup.sh
0 23 * * 1-6 /usr/local/bin/incrementalbackup.sh
```

Això significa:
- **Diumenges (0)** a les 23:00 → backup complet
- **Dilluns a dissabte (1-6)** a les 23:00 → backup incremental

![Visualització del crontab final](/tasca02/img_t02_lin/captura32.png)

---

## Verificació final

Abans de confiar en l'automatització, provem manualment els scripts per verificar que funcionen correctament.

### Prova del script de backup complet:

![Execució manual de fullbackup.sh](/tasca02/img_t02_lin/captura33.png)

El script s'executa correctament, creant un nou backup complet de 214 MB en poc menys de 5 segons.

### Prova del script de backup incremental:

![Execució manual de incrementalbackup.sh](/tasca02/img_t02_lin/captura34.png)

El backup incremental detecta petits canvis (68 KB de nous fitxers i 1,09 MB de canvis) i s'executa en només 0,28 segons.

---

## Conclusió 

Aquesta guia ha demostrat com implementar un sistema de còpies de seguretat complet i automatitzat en un servidor Linux utilitzant **Duplicity**. S'ha cobrit tot el procés des de la configuració inicial del disc de backup fins a l'automatització total amb scripts i cron.

El sistema resultant ofereix:
- **Backups complets setmanals** els diumenges a les 23:00
- **Backups incrementals diaris** de dilluns a dissabte a la mateixa hora
- **Xifratge GPG** integrat per a la confidencialitat de les dades
- **Seguretat millorada** mitjançant el muntatge/desmuntatge dinàmic del suport de backup
- **Verificació i restauració** fiables mitjançant proves pràctiques

La combinació de Duplicity amb scripts modulars (`fullbackup.sh` i `incrementalbackup.sh`) i el programador cron permet una gestió de backups eficient, fiable i adaptada a entorns de producció, assegurant la protecció contínua dels fitxers crítics amb mínima intervenció manual.

---

[Guia de Windows](GuiaWindows.md)  

[Explicació de la tasca](README.md)  

[Tornar a la pàgina principal](../)
