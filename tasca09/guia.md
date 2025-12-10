# **T09: Servidor fitxers Linux. NFS - Guia de Configuració**

## **Introducció**
En aquesta tasca implementarem un servidor de fitxers NFS (Network File System) per al client DevOptimize Solutions, una startup de desenvolupament de programari que necessita centralitzar el seu codi font i actius. El NFS és la solució nativa per a entorns Linux que permet compartir recursos de fitxers a través de la xarxa.

## **Fase 1: Preparació del Servidor**

### **Actualització del sistema**
```bash
sudo apt update && sudo apt upgrade -y
```

![Actualització del sistema](/tasca09/img_t09/captura1.png)

![Actualització del sistema](/tasca09/img_t09/captura2.png)

Abans de començar qualsevol instal·lació, és essencial actualitzar el sistema per assegurar-nos que tenim les últimes versions dels paquets i correccions de seguretat. Aquest pas evita possibles conflictes amb dependències obsoletes.

### **Configuració de xarxa**
```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

![Configuració de xarxa](/tasca09/img_t09/captura3.png)

Configurar correctament les interfícies de xarxa és crucial per al funcionament del NFS. En aquest cas, s'han configurat dues interfícies: una per a Internet (dhcp4: true) i una per a la xarxa interna entre servidor i client.

### **Creació de grups**
```bash
sudo groupadd devs
sudo groupadd admins
```

![Creació de grups](/tasca09/img_t09/captura5.png)

Es creen dos grups: `devs` per als desenvolupadors i `admins` per als administradors. Aquesta separació permet aplicar polítiques de permisos diferents per a cada rol, seguint el principi de mínim privilegi.

### **Creació d'usuaris**
```bash
sudo useradd -m -s /bin/bash -G devs dev01
sudo useradd -m -s /bin/bash -G admins admin01
```

![Creació d'usuaris](/tasca09/img_t09/captura6.png)

Es creen els usuaris `dev01` i `admin01` afegint-los als grups corresponents. L'opció `-m` crea el directori home, `-s /bin/bash` assigna el shell per defecte, i `-G` especifica els grups secundaris.

### **Assignació de contrasenyes**
```bash
sudo passwd dev01
sudo passwd admin01
```

![Assignació contrasenya dev01](/tasca09/img_t09/captura7.png)
![Assignació contrasenya admin01](/tasca09/img_t09/captura8.png)

Assignar contrasenyes als usuaris és necessari per poder iniciar sessió. És important utilitzar contrasenyes segures i diferents per a cada usuari.

### **Creació de directoris compartits**
```bash
sudo mkdir -p /srv/nfs/dev_projects
sudo mkdir -p /srv/nfs/admin_tools
```

![Creació directoris](/tasca09/img_t09/captura9.png)

Es creen els directoris que es compartiran via NFS. La ruta `/srv/nfs/` és la ubicació estàndard per a recursos compartits de xarxa. L'estructura separada permet organitzar diferents tipus de contingut.

### **Configuració de permisos**
```bash
sudo chown root:devs /srv/nfs/dev_projects
sudo chmod 770 /srv/nfs/dev_projects
sudo chown root:admins /srv/nfs/admin_tools
sudo chmod 770 /srv/nfs/admin_tools
```

![Permisos dev_projects](/tasca09/img_t09/captura10.png)
![Permisos admin_tools](/tasca09/img_t09/captura11.png)

Els permisos 770 (`rwxrwx---`) signifiquen que només el propietari (root) i el grup assignat tenen accés complet, mentre que altres usuaris no tenen cap accés. Això assegura que només els membres dels grups corresponents puguin accedir als seus directoris.

## **Fase 2: Instal·lació i Configuració del Servei NFS**

### **Instal·lació del servidor NFS**
```bash
sudo apt install nfs-kernel-server
```

![Instal·lació NFS](/tasca09/img_t09/captura12.png)

S'instal·la el paquet `nfs-kernel-server` que proporciona la funcionalitat de servidor NFS. El sistema també instal·la automàticament les dependències necessàries com `nfs-common`, `rpcbind`, i `keyutils`.

![Proces instal·lació](/tasca09/img_t09/captura13.png)

Durant la instal·lació, es creen automàticament diversos fitxers de configuració i unitats de systemd necessàries per al funcionament del servei.

### **Configuració de les exportacions**
```bash
sudo nano /etc/exports
```

![Configuració /etc/exports](/tasca09/img_t09/captura14.png)

El fitxer `/etc/exports` és el punt central de configuració del NFS, on es defineixen quins directoris es comparteixen i amb quines opcions.

```bash
/srv/nfs/admin_tools 192.168.56.0/24(rw,sync,no_subtree_check)
/srv/nfs/dev_projects 192.168.56.0/24(rw,sync,no_subtree_check)
```

![Contingut exports](/tasca09/img_t09/captura15.png)

Cada línia especifica:
- **Ruta**: Directori a compartir
- **Xarxa**: 192.168.56.0/24 (tots els hosts d'aquesta xarxa)
- **Opcions**: 
  - `rw`: Lectura i escriptura
  - `sync`: Escriu canvis al disc abans de respondre
  - `no_subtree_check`: Millor rendiment però menys segur

### **Inici i habilitació del servei**
```bash
sudo systemctl start nfs-kernel-server
sudo systemctl reload nfs-kernel-server
sudo systemctl enable nfs-kernel-server
```

![Inici servei NFS](/tasca09/img_t09/captura16.png)
![Habilitació servei](/tasca09/img_t09/captura18.png)

- `start`: Inicia el servei immediatament
- `reload`: Recarrega la configuració sense aturar el servei
- `enable`: Configura el servei per iniciar-se automàticament en l'arrencada

### **Verificació de les exportacions**
```bash
sudo exportfs -u
```

![Verificació exportacions](/tasca09/img_t09/captura17.png)

La comanda `exportfs -u` mostra totes les exportacions configurades. Es pot veure que ambdues carpetes estan disponibles per a tota la xarxa 192.168.56.0/24.

### **Verificació de l'estat del servei**
```bash
sudo systemctl status nfs-kernel-server
```

![Estat servei NFS](/tasca09/img_t09/captura19.png)

El servei està actiu i habilitat. Les advertències sobre `no_subtree_check` són normals i informen sobre el comportament per defecte de les versions actuals de nfs-utils.

### **Verificació dels grups creats**
```bash
getent group devs
getent group admins
```

![Verificació grups](/tasca09/img_t09/captura20.png)

La comanda `getent group` mostra la informació dels grups del sistema. Es pot veure que:
- `devs` té GID 1001 i conté l'usuari `dev01`
- `admins` té GID 1002 i conté l'usuari `admin01`

Aquests GID seran importants per a la sincronització amb el client NFS.

---

## **Fase 3: Configuració del Client NFS**

### **Configuració de xarxa del client**

![Configuració adaptador 1](/tasca09/img_t09/captura21.png)
El client està configurat amb dos adaptadors de xarxa:
- **Adaptador 1**: Connectat a Xarxa NAT, permet l'accés a Internet per actualitzar paquets i descarregar software.
- **Adaptador 2**: Adaptador només-amfitrió, per a la comunicació interna amb el servidor.

![Configuració adaptador 2](/tasca09/img_t09/captura22.png)
L'adaptador només-amfitrió utilitza la interfície VirtualBox Host-Only Ethernet Adapter, creant una xarxa privada entre el servidor i el client sense interferències externes.

### **Verificació de connectivitat**
```bash
ping 192.168.56.112
```

![Ping al servidor](/tasca09/img_t09/captura23.png)

El ping demostra que hi ha connectivitat entre client i servidor amb temps de resposta baixos (0.75-1.24 ms), indicant una xarxa local saludable. El 0% de pèrdua de paquets confirma l'estabilitat de la connexió.

### **Actualització del sistema client**
```bash
sudo apt update && sudo apt upgrade
```

![Actualització client](/tasca09/img_t09/captura24.png)

Abans d'instal·lar el client NFS, s'actualitza el sistema per assegurar compatibilitat i seguretat. S'identifiquen 237 paquets actualitzables, la qual cosa és normal en un sistema que no s'ha actualitzat recentment.

![Procés d'actualització](/tasca09/img_t09/captura25.png)

Durant l'actualització, es configuren múltiples components del sistema incloent Ruby, biblioteques de xarxa, i paquets de seguretat. La regeneració del initramfs és necessària perquè el sistema pugui arrencar correctament després d'actualitzacions del kernel.

### **Instal·lació del client NFS**
```bash
sudo apt install nfs-common
```

![Instal·lació nfs-common](/tasca09/img_t09/captura26.png)

El paquet `nfs-common` conté totes les eines necessàries per al client NFS, incloent:
- `showmount`: Per llistar recursos compartits
- `mount.nfs`: Per muntar recursos NFS
- `nfsstat`: Per estadístiques d'ús
Les dependències instal·lades inclouen `keyutils` per a autenticació, `libnfsidmap1` per a mapatge d'identificadors, i `rpcbind` per a comunicació RPC.

![Configuració del client NFS](/tasca09/img_t09/captura27.png)

Durant la instal·lació es creen els fitxers de configuració `/etc/idmapd.conf` i `/etc/nfs.conf`, i es crea l'usuari del sistema `statd` amb UID 130. Les unitats de systemd creades permeten la gestió automatitzada dels serveis NFS del client.

### **Verificació de recursos compartits**
```bash
sudo showmount -e 192.168.56.112
```

![Llistat d'exportacions](/tasca09/img_t09/captura28.png)

La comanda `showmount` mostra que el servidor comparteix dos recursos:
- `/srv/nfs/dev_projects`
- `/srv/nfs/admin_tools`
El marcador "(everyone)" indica que aquests recursos estan disponibles per a tota la xarxa 192.168.56.0/24.

### **Creació de grups amb GID específics**
```bash
sudo groupadd -g 1001 devs
sudo groupadd -g 1002 admins
```

![Creació grups amb GID](/tasca09/img_t09/captura29.png)

És **crític** que els GID coincideixin exactament entre servidor i client. El NFS identifica usuaris i grups pels seus identificadors numèrics (UID/GID), no pels noms. Si els números no coincideixen, els permisos no funcionaran correctament.

### **Creació d'usuaris amb UID específics**
```bash
sudo useradd -m -u 1001 -g devs dev01
sudo useradd -m -u 1002 -g admins admin01
```

![Creació usuaris amb UID](/tasca09/img_t09/captura30.png)

Els UID també han de coincidir amb els del servidor:
- `dev01`: UID 1001, GID 1001 (grup devs)
- `admin01`: UID 1002, GID 1002 (grup admins)
Aquesta sincronització assegura que quan aquests usuaris accedeixin als recursos compartits, el servidor els reconegui correctament.

### **Assignació de contrasenyes**
```bash
sudo passwd dev01
sudo passwd admin01
```

![Assignació contrasenyes](/tasca09/img_t09/captura31.png)

El sistema obliga a utilitzar contrasenyes amb almenys 8 caràcters, seguint bones pràctiques de seguretat. Contrasenyes febles són un punt d'entrada comú per a atacs de força bruta.

### **Creació de punts de muntatge**
```bash
sudo mkdir -p /mnt/admin_tools
sudo mkdir -p /mnt/dev_projects
```

![Creació punt muntatge admin_tools](/tasca09/img_t09/captura32.png)
![Creació punt muntatge dev_projects](/tasca09/img_t09/captura39.png)

Els directoris `/mnt/` són la ubicació estàndard per a dispositius i recursos muntats temporalment. La separació en dos directoris diferents permet organitzar l'accés als diferents tipus de recursos.

### **Muntatge del recurs admin_tools**
```bash
sudo mount -t nfs 192.168.56.112:/srv/nfs/admin_tools /mnt/admin_tools
```

![Muntatge admin_tools](/tasca09/img_t09/captura33.png)

La comanda `mount` estableix la connexió entre el recurs remot i el directori local. L'opció `-t nfs` especifica el tipus de sistema de fitxers, i la sintaxi `servidor:ruta` indica quin recurs muntar.

### **Prova d'escriptura al recurs muntat**
```bash
sudo touch /mnt/admin_tools/proval.txt
```

![Creació fitxer de prova](/tasca09/img_t09/captura34.png)

La creació d'un fitxer com a root demostra que el muntatge funciona i que tenim permisos d'escriptura. Aquest test valida que la connexió NFS està activa i que les opcions `rw` (read-write) a `/etc/exports` estan funcionant.

### **Verificació del fitxer creat**
```bash
ls -l /mnt/admin_tools
```

![Verificació fitxer](/tasca09/img_t09/captura35.png)

El fitxer `proval.txt` apareix amb propietari `root:root` i permisos 644. Aquest és el comportament esperat quan s'utilitza `root_squash` (opció per defecte), que converteix operacions de root del client en usuari nobody al servidor per seguretat.

### **Prova de remuntatge**
```bash
sudo umount /mnt/admin_tools
sudo mount -t nfs 192.168.56.112:/srv/nfs/admin_tools /mnt/admin_tools
sudo touch /mnt/admin_tools/prova2.txt
```

![Desmuntatge](/tasca09/img_t09/captura36.png)
![Remuntatge](/tasca09/img_t09/captura37.png)
![Segona prova escriptura](/tasca09/img_t09/captura38.png)

Aquest procés de desmuntar i tornar a muntar demostra:
1. La fluïdesa en la gestió de recursos NFS
2. Que els canvis es persisteixen al servidor
3. La capacitat de restablir connexions sense reiniciar serveis

### **Interfície gràfica del client**
![Interfície gràfica](/tasca09/img_t09/captura40.png)

La interfície gràfica del Zorin OS mostra les ubicacions favorites i permet navegar pels recursos muntats. Aquesta és la forma en què els usuaris finals (desenvolupadors i administradors) interactuaran amb els recursos compartits.

---

## **Fase 4: Problemes d'Accés i Configuració Automàtica**

### **Problema d'accés des de l'explorador gràfic**
![Error permisos explorador](/tasca09/img_t09/captura41.png)

L'explorador gràfic mostra un error de permisos quan intenta accedir a `/mnt/admin_tools`. Aquest problema succeeix perquè:
1. Els directoris muntats via NFS hereden els permisos del servidor
2. L'explorador gràfic s'executa amb l'usuari `client` (UID 1000)
3. Els directoris al servidor tenen permisos 770 (`rwxrwx---`)
4. L'usuari `client` no és membre dels grups `devs` o `admins` al servidor

Això demostra una limitació important del NFS sense autenticació centralitzada: els permisos depenen exclusivament dels UID/GID, i si no coincideixen exactament, l'accés es denega.

### **Configuració de muntatge automàtic**
```bash
sudo nano /etc/fstab
```

![Edició /etc/fstab](/tasca09/img_t09/captura42.png)

El fitxer `/etc/fstab` (File System Table) conté la informació dels sistemes de fitxers que s'han de muntar automàticament durant l'arrencada del sistema.

![Configuració /etc/fstab](/tasca09/img_t09/captura43.png)

S'afegeixen dues línies al final del fitxer:
```bash
192.168.56.112:/srv/nfs/admin_tools /mnt/admin_tools nfs defaults 0 0
192.168.56.112:/srv/nfs/dev_projects /mnt/dev_projects nfs defaults 0 0
```

**Explicació de cada camp**:
1. **Fitxer de sistema**: `192.168.56.112:/srv/nfs/admin_tools` - Recurs NFS remot
2. **Punt de muntatge**: `/mnt/admin_tools` - Directori local on es muntarà
3. **Tipus**: `nfs` - Especifica que és un sistema de fitxers NFS
4. **Opcions**: `defaults` - Inclou rw, suid, dev, exec, auto, nouser, async
5. **Dump**: `0` - No fer còpies de seguretat amb dump
6. **Pass**: `0` - No verificar amb fsck en l'arrencada

### **Reinici del sistema client**
```bash
reboot
```

![Reinici client](/tasca09/img_t09/captura44.png)

El reinici és necessari per provar la configuració de muntatge automàtic. Durant l'arrencada, el sistema llegeix `/etc/fstab` i intenta muntar tots els sistemes de fitxers especificats.

### **Verificació de muntatges després del reinici**
```bash
mount | grep nfs
```

![Verificació muntatges NFS](/tasca09/img_t09/captura45.png)

Després del reinici, es comprova que els recursos NFS s'han muntat automàticament. La sortida mostra:
- **Ambdós recursos estan muntats**: `/srv/nfs/admin_tools` i `/srv/nfs/dev_projects`
- **Versió NFS**: `vers=4.2` - S'utilitza la versió 4.2 del protocol
- **Opcions**: `rw,relative` - Lectura/escriptura amb temps relatius
- **Mides**: `rsize=524288,wsize=524288` - Buffers de 512KB per optimitzar transferències
- **Seguretat**: `sec=sys` - Autenticació del sistema (basada en UID/GID)
- **Adreces**: `clientaddr=192.168.56.113`, `addr=192.168.56.112` - Confirmació de la connexió entre client i servidor

### **Visualització dels directoris muntats a l'explorador**
![Explorador mostrant directoris muntats](/tasca09/img_t09/captura46.png)
![Explorador després del reinici](/tasca09/img_t09/captura47.png)

L'explorador gràfic ara mostra les dues carpetes muntades (`admin_tools` i `dev_projects`) a la ubicació `/mnt/`. Tot i que l'explorador pot mostrar les carpetes, l'accés al contingut encara pot estar restringit per permisos.

### **Pantalla d'inici de sessió del client**
![Pantalla d'inici de sessió](/tasca09/img_t09/captura48.png)

La pantalla d'inici de sessió mostra els tres usuaris configurats:
1. **client**: L'usuari principal del sistema client
2. **admin01**: Usuari creat per a l'administració (grup admins)
3. **dev01**: Usuari creat per al desenvolupament (grup devs)

Aquests usuaris poden iniciar sessió individualment per provar els permisos d'accés als recursos NFS des de diferents contexts d'usuari.

## **Anàlisi i Conclusions**

### **Èxits de la implementació**:
1. **Servidor NFS configurat correctament**: Exporta dos directoris amb permisos específics
2. **Client NFS funcionant**: Pot muntar i accedir als recursos compartits
3. **Sincronització UID/GID**: Els identificadors coincideixen entre servidor i client
4. **Muntatge automàtic configurat**: Els recursos es munten automàticament en l'arrencada
5. **Connectivitat verificada**: Ping i muntatges funcionen sense errors de xarxa

### **Limitacions identificades**:
1. **Gestió manual d'usuaris**: Cal crear usuaris manualment a totes les màquines
2. **Depenència de UID/GID**: Els permisos fallen si els identificadors no coincideixen
3. **Seguretat bàsica**: Sense autenticació centralitzada o xifrat
4. **Problemes amb explorador gràfic**: L'accés des de GUI pot tenir problemes de permisos

### **Avaluació de la solució**:
La implementació demostra que el NFS és una solució viable per a centralitzar fitxers en entorns Linux, però requereix una gestió acurada dels permisos i identificadors d'usuari. Per a entorns petits com DevOptimize Solutions, aquesta solució pot ser suficient, però per a creixement futur es recomana migrar a una infraestructura més robusta amb autenticació centralitzada.

**La prova de concepte està completada amb èxit**: El client pot accedir als recursos compartits, els permisos funcionen segons els grups, i el muntatge automàtic assegura disponibilitat constant dels recursos.

---

[Explicació de la tasca](README.md)

---

[Tornar a la pàgina principal](../)
