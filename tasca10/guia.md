# Tasca 10: Servidor d'Impressió Linux. CUPS (Prova de Concepte)

En aquesta guia implementarem un servidor d'impressió centralitzat utilitzant CUPS (Common Unix Printing System) a Ubuntu Server. Configurarem una impressora virtual PDF compartida a la xarxa per demostrar la gestió centralitzada d'impressió als clients.

## 1. Configuració inicial del servidor Ubuntu

### Configuració de xarxa del servidor - Adaptador NAT

Abans de començar amb la instal·lació, verifiquem la configuració de xarxa de la nostra màquina virtual del servidor.

![Configuració de xarxa NAT del servidor](/tasca10/img_t10/captura1.png)

**Anàlisi**: El servidor està configurat amb:
- **Adaptador 1**: Connectat a una xarxa NAT
- **Nom de la xarxa**: NatNetwork
- **Tipus d'adaptador**: Intel PRO/1000 MT Desktop (82540EM)
- **Adreça MAC**: 0800273CA793
- **Estat**: Cable connectat

Aquesta configuració permet al servidor tenir accés a Internet a través de la xarxa NAT per descarregar paquets, mentre manté una adreça IP privada.

### Configuració de xarxa del servidor - Adaptador Host-Only

![Configuració de xarxa Host-Only del servidor](/tasca10/img_t10/captura2.png)

**Anàlisi**: El servidor també té un segon adaptador configurat:
- **Adaptador 2**: Mode "només l'amfitrió" (Host-Only)
- **Nom de l'adaptador**: VirtualBox Host-Only Ethernet Adapter
- **Tipus d'adaptador**: Intel PRO/1000 MT Desktop (82540EM)
- **Adreça MAC**: 08002705C704

Aquest tipus de configuració crea una xarxa privada entre la màquina virtual i l'ordinador amfitrió, ideal per a comunicacions internes entre màquines virtuals.

### Verificació de la configuració de xarxa del servidor

```bash
ip a
```

![Verificació de la configuració de xarxa del servidor](/tasca10/img_t10/captura3.png)

**Anàlisi**: La comanda `ip a` mostra totes les interfícies de xarxa del servidor:
- **lo**: Loopback amb adreça 127.0.0.1/8
- **enp0s3**: Xarxa NAT amb adreça 10.0.2.16/24 (Adreça MAC: 08:00:27:3c:a7:93)
- **enp0s8**: Xarxa Host-Only amb adreça 192.168.56.114/24 (Adreça MAC: 08:00:27:d5:c7:d4)

El servidor té dues IPs: una a la xarxa NAT (10.0.2.16) i una altra a la xarxa Host-Only (192.168.56.114).

## 2. Configuració del client Ubuntu Desktop

### Verificació de la configuració de xarxa del client

```bash
ip a
```

![Verificació de la configuració de xarxa del client](/tasca10/img_t10/captura4.png)

**Anàlisi**: El client també té una configuració de xarxa similar:
- **enp0s3**: Xarxa NAT amb adreça 10.0.2.14/24 (Adreça MAC: 08:00:27:ea:6b:d9)
- **enp0s8**: Xarxa Host-Only amb adreça 192.168.56.113/24 (Adreça MAC: 08:00:27:c1:d0:0a)

Aquestes adreces confirmen que el client i el servidor estan a la mateixa xarxa Host-Only (192.168.56.0/24), permetent la comunicació directa entre ells.

### Prova de connectivitat des del servidor al client

```bash
ping 192.168.56.113
```

![Prova de ping des del servidor al client](/tasca10/img_t10/captura5.png)

**Anàlisi**: Fem un test de connectivitat des del servidor al client:
- **Paquets enviats**: 6
- **Paquets rebuts**: 6
- **Pèrdua de paquets**: 0%
- **Temps de resposta**: Mitjana de 0.532 ms (mínim 0.324 ms, màxim 0.856 ms)

Això confirma que el servidor pot arribar al client sense problemes, amb temps de resposta molt baixos.

### Prova de connectivitat des del client al servidor

```bash
ping 192.168.56.114
```

![Prova de ping des del client al servidor](/tasca10/img_t10/captura6.png)

**Anàlisi**: Fem també el ping en sentit invers, des del client al servidor:
- **Paquets enviats**: 7
- **Paquets rebuts**: 7
- **Pèrdua de paquets**: 0%
- **Temps de resposta**: Mitjana de 0.766 ms (mínim 0.492 ms, màxim 1.000 ms)

La connectivitat bidireccional està confirmada, essent essencial per al funcionament del servidor d'impressió.

## 3. Preparació del sistema servidor

### Actualització del sistema servidor

Abans d'instal·lar qualsevol servei, sempre actualitzem el sistema per assegurar-nos de tenir les versions més recents i segures dels paquets.

```bash
sudo apt update && sudo apt upgrade -y
```

![Inici de l'actualització del sistema](/tasca10/img_t10/captura7.png)

**Anàlisi**: La comanda `apt update && sudo apt upgrade -y` fa dues coses:
1. `apt update`: Actualitza la llista de paquets disponibles dels repositoris
2. `apt upgrade -y`: Actualitza tots els paquets instal·lats, amb el paràmetre `-y` que respon automàticament "sí" a les preguntes de confirmació

### Procés d'actualització en curs

![Procés d'actualització en curs](/tasca10/img_t10/captura8.png)

**Anàlisi**: Durant l'actualització, veiem que:
- **Repositoris utilitzats**: Ubuntu noble (24.04), noble-updates, noble-security
- **Paquets per actualitzar**: 59 paquets disponibles per actualització
- **Traduccions descarregades**: 1,759 kB de traduccions al castellà
- **Paquets que s'actualitzaran**: 33 paquets específics seran actualitzats

El sistema ens pregunta si volem continuar, ja que necessitarà descarregar 46,0 MB d'arxius i utilitzar 3,938 kB addicionals d'espai de disc.

### Finalització de l'actualització

![Finalització de l'actualització](/tasca10/img_t10/captura9.png)

**Anàlisi**: L'actualització s'ha completat correctament. Veiem que:
- S'han processat diversos "disparadors" (triggers) per a serveis com rsyslog, man-db, dbus, udev, etc.
- El nucli del sistema ja està actualitzat ("Running kernel seems to be up-to-date")
- S'han reiniciat alguns serveis automàticament
- S'identifica que hi ha sessions d'usuari executant binaris desactualitzats (això és normal i no afecta la nostra configuració)

## 4. Instal·lació del servidor CUPS

### Inici de la instal·lació de CUPS

```bash
sudo apt install cups -y
```

![Inici de la instal·lació de CUPS](/tasca10/img_t10/captura10.png)

**Anàlisi**: Iniciem la instal·lació del paquet `cups`, que és el servidor d'impressió que farem servir. CUPS (Common Unix Printing System) és el sistema d'impressió estàndard per a sistemes Unix/Linux, desenvolupat originalment per Apple i ara mantingut per OpenPrinting.

### Procés d'instal·lació de CUPS

![Procés d'instal·lació de CUPS](/tasca10/img_t10/captura11.png)

**Anàlisi**: *Nota: Aquesta és la captura 11 que esmentaves, que mostra el procés d'instal·lació del paquet cups.*

### Finalització de la instal·lació de CUPS

![Finalització de la instal·lació de CUPS](/tasca10/img_t10/captura12.png)

**Anàlisi**: La instal·lació de CUPS s'ha completat amb èxit. Destacat:
- **Usuari del sistema creat**: `cups-browsed` amb UID 113 i grup `lpadmin`
- **Servei configurat**: S'ha creat un enllaç simbòlic per al servei systemd
- **Configuracions processades**: cups-core-drivers i cups
- **PPD actualitzats**: S'han actualitzat els fitxers PPD (PostScript Printer Description) per a cups i cups-filters
- **Disparadors processats**: libc-bin i udev

El servei està preparat per funcionar, amb totes les configuracions necessàries aplicades.

## 5. Configuració i gestió del servei CUPS

### Verificació de l'estat del servei CUPS

```bash
sudo systemctl status cups
```

![Estat del servei CUPS](/tasca10/img_t10/captura13.png)

**Anàlisi**: El servei CUPS està actiu i funcionant correctament:
- **Estat**: "active (running)" des de fa 1 minut i 11 segons
- **Configuració**: Carregat des de `/usr/lib/systemd/system/cups.service`
- **TriggeredBy**: cups.socket i cups.path (s'activa automàticament quan es necessita)
- **PID principal**: 6339 (el procés cupsd)
- **Estat del planificador**: "Scheduler is running..."
- **Memòria utilitzada**: 2.9M (pic de 16.7M)
- **CPU utilitzada**: 302ms

Això confirma que el servei s'ha iniciat correctament i està llest per rebre peticions.

### Reinici del servei CUPS

```bash
sudo systemctl restart cups && sudo systemctl status cups
```

![Reinici del servei CUPS](/tasca10/img_t10/captura14.png)

**Anàlisi**: Reiniciem el servei CUPS i immediatament verifiquem el seu estat. Veiem que:
- El servei s'ha reiniciat correctament (nova data/hora: Wed 2025-12-17 17:03:03 UTC)
- **Nou PID**: 6637 (confirmant que s'ha llançat una nova instància)
- **Estat**: Continua "active (running)"
- **Memòria**: 1.7M (una mica menys que abans)
- **Temps d'inici**: Només 13ms des del reinici

El reinici serveix per aplicar possibles canvis de configuració i assegurar-nos que el servei està fresc.

## 6. Instal·lació de la impressora virtual PDF

### Inici de la instal·lació de cups-pdf

```bash
sudo apt install cups-pdf
```

![Inici de la instal·lació de cups-pdf](/tasca10/img_t10/captura15.png)

**Anàlisi**: Comencem la instal·lació de `cups-pdf`, que és el paquet que proporciona el backend per a la impressora virtual PDF. Aquest paquet permet "imprimir" documents a fitxers PDF en comptes de paper físic.

### Procés d'instal·lació de cups-pdf

![Procés d'instal·lació de cups-pdf](/tasca10/img_t10/captura16.png)

**Anàlisi**: El sistema detecta que `cups-pdf` és proporcionat pel paquet `printer-driver-cups-pdf` i el selecciona automàticament:
- **Paquet a instal·lar**: printer-driver-cups-pdf
- **Mida de descàrrega**: 25,5 kB (molt petit)
- **Espai necessari**: 241 kB addicionals
- **Versió**: 3.0.1-14build2 per a arquitectura amd64

També suggereix `system-config-printer` com a paquet opcional per a la gestió gràfica d'impressores.

### Creació de còpia de seguretat de la configuració CUPS

Abans de modificar la configuració, sempre és bona pràctica fer una còpia de seguretat.

```bash
sudo cp /etc/cups/cupsd.conf /etc/cups/cupsd.conf.bak
```

![Còpia de seguretat del fitxer de configuració](/tasca10/img_t10/captura17.png)

**Anàlisi**: Creem una còpia de seguretat del fitxer de configuració principal de CUPS. Això ens permet:
- Revertir canvis si alguna cosa no funciona
- Comparar configuracions abans i després
- Mantenir una versió coneguda que funciona

El fitxer `cupsd.conf.bak` servirà com a punt de restauració si necessitem tornar a una configuració que sabem que funciona.

### Edició del fitxer de configuració CUPS

```bash
sudo nano /etc/cups/cupsd.conf
```

![Edició del fitxer cupsd.conf](/tasca10/img_t10/captura18.png)

**Anàlisi**: Obrim l'editor nano per modificar el fitxer de configuració principal de CUPS. Cal modificar aquest fitxer per:
1. Permetre l'accés des de la xarxa local
2. Configurar el servidor per escoltar en totes les interfícies
3. Activar la detecció de impressores compartides

## 7. Configuració de l'accés remot a CUPS

### Configuració dels ports d'escolta

![Configuració dels ports d'escolta](/tasca10/img_t10/captura19.png)

**Anàlisi**: Aquesta part del fitxer de configuració controla on escolta el servidor CUPS:
- **Listen 631**: Fa que CUPS escolti al port 631 (port estàndard IPP) en totes les interfícies
- **Listen /run/cups/cups.sock**: També escolta en el socket Unix local per a connexions locals
- **Browsing On**: Activa la detecció de impressores a la xarxa
- **BrowseLocalProtocols dnssd**: Utilitza DNS-SD (DNS Service Discovery) per anunciar les impressores

Aquesta configuració permet que els clients a la xarxa local detectin i s'connectin al servidor d'impressió.

### Configuració de les polítiques d'accés

![Configuració de les polítiques d'accés](/tasca10/img_t10/captura20.png)

**Anàlisi**: Aquesta secció defineix qui pot accedir al servidor i a les seves funcions:
1. **Accés general al servidor** (`<Location />`):
   - `Order allow,deny`: Processa primer les regles allow i després deny
   - `Allow @LOCAL`: Permet l'accés des de la xarxa local (definit per les interfícies de xarxa locals)

2. **Accés a les pàgines d'administració** (`<Location /admin>`):
   - `AuthType Default`: Requereix autenticació
   - `Require valid-user`: Només usuaris vàlids poden accedir
   - `Order allow,deny`: Processa les regles en ordre
   - `Allow @LOCAL`: Permet l'accés des de la xarxa local, però amb autenticació

Aquesta configuració equilibra seguretat i accessibilitat, permetent que els clients de la xarxa local utilitzin el servidor d'impressió mentre es protegeix l'accés administratiu.

### Reinici del servei CUPS després dels canvis

Després de modificar el fitxer de configuració `cupsd.conf`, necessitem reiniciar el servei per aplicar els canvis.

```bash
sudo systemctl restart cups
```

![Reinici del servei CUPS](/tasca10/img_t10/captura21.png)

**Anàlisi**: Reiniciem el servei CUPS amb la comanda `sudo systemctl restart cups`. Aquest reinici aplica totes les modificacions que hem fet al fitxer `cupsd.conf`, incloent la configuració d'accés remot i els ports d'escolta.

### Verificació de l'estat després del reinici

```bash
sudo systemctl status cups
```

![Estat del servei CUPS després del reinici](/tasca10/img_t10/captura22.png)

**Anàlisi**: Després del reinici, verifiquem que el servei està actiu i funcionant:
- **Estat**: "active (running)" des de fa 19 segons
- **Nou PID**: 6932 (confirmant que s'ha llançat una nova instància)
- **Tasques**: 2 (abans n'hi havia 1)
- **Memòria**: 2.3M (pic de 2.6M)
- **CPU**: 16ms

El servei s'ha reiniciat correctament i està llest per acceptar connexions remotes.

## 9. Accés a la interfície web d'administració

### Accés via HTTPS al servidor CUPS

![Accés via HTTPS a la interfície web](/tasca10/img_t10/captura23.png)

**Anàlisi**: Ara accedim a la interfície web d'administració de CUPS utilitzant el navegador web. La URL és `https://192.168.56.114:631`, on:
- **192.168.56.114**: És l'adreça IP del servidor a la xarxa Host-Only
- **631**: És el port estàndard per a CUPS/IPP
- **https**: El protocol segur (nota que mostra "No és segur" perquè utilitza un certificat autosignat)

Aquest accés és possible gràcies a les modificacions que vam fer al fitxer `cupsd.conf` que permeten connexions des de la xarxa local.

### Pantalla d'inici de CUPS

![Pantalla d'inici de CUPS 2.4.7](/tasca10/img_t10/captura24.png)

**Anàlisi**: Aquesta és la pàgina principal de la interfície web de CUPS versió 2.4.7. La interfície mostra:
- **Descripció**: CUPS és el sistema d'impressió de codi obert basat en estàndards desenvolupat per OpenPrinting
- **Funcionalitats principals**:
  - CUPS per a usuaris: Descripció i impressió des de línia de comandes
  - CUPS per a administradors: Afegir impressores, gestió de polítiques, impressores de xarxa
  - CUPS per a desenvolupadors: Manuals de programació

### Secció d'administració

![Secció d'administració de CUPS](/tasca10/img_t10/captura25.png)

**Anàlisi**: Accedim a la secció d'administració on podem gestionar tot el sistema d'impressió:
- **Impressores**: Opcions per afegir, trobar o gestionar impressores
- **Classes**: Per afegir o gestionar classes d'impressores (agrupacions)
- **Treballs**: Per administrar les cues d'impressió
- **Servidor**: Configuració del servidor amb opcions com:
  - Editar l'arxiu de configuració
  - Compartir impressores connectades
  - Permetre administració remota (activat)
  - Altres opcions de seguretat

## 10. Configuració dels permisos d'administració

### Intenta afegir una impressora (sense permisos)

![Error d'afegir impressora sense permisos](/tasca10/img_t10/captura26.png)

**Anàlisi**: Quan intentem afegir una impressora des de la interfície web, rebem un error "Prohibit". Això succeeix perquè l'usuari actual no té els permisos necessaris per realitzar accions administratives a CUPS. Per solucionar-ho, necessitem afegir l'usuari al grup `lpadmin`.

### Afegir l'usuari al grup lpadmin

```bash
sudo usermod -aG lpadmin usuari
```

![Afegir usuari al grup lpadmin](/tasca10/img_t10/captura27.png)

**Anàlisi**: La comanda `sudo usermod -aG lpadmin usuari` afegeix l'usuari "usuari" al grup "lpadmin". El paràmetre `-aG` significa:
- **-a**: Append (afegeix sense treure d'altres grups)
- **-G**: Grup secundari
Això assegura que l'usuari manté la seva pertinença a altres grups mentre s'afegeix al grup administratiu d'impressió.

### Verificació dels grups de l'usuari

```bash
id usuari
```

![Verificació dels grups de l'usuari](/tasca10/img_t10/captura28.png)

**Anàlisi**: Verifiquem que l'usuari "usuari" està ara correctament afegit al grup `lpadmin`. La sortida mostra:
- **UID**: 1000(usuari)
- **GID principal**: 1000(usuari)
- **Grups secundaris**: 4(adm), 24(cdrom), 27(sudo), 30(dip), 46(plugdev), 101(lxd), **116(lpadmin)**
L'usuari ara té els permisos necessaris per administrar impressores a través de la interfície web de CUPS.

## 11. Configuració de la impressora virtual

### Inici del procés d'afegir impressora

![Selecció del tipus d'impressora](/tasca10/img_t10/captura29.png)

**Anàlisi**: Ara que tenim permisos, comencem el procés d'afegir una impressora. Veiem:
- **Impressores locals**: Inclou CUPS-PDF (Virtual PDF Printer)
- **Impressores en xarxa descobades**: Actualment buit
- **Altres impressores en xarxa**: Diferents protocols com IPP, LPD/LPR, AppSocket/HP JetDirect

Seleccionem "CUPS-PDF (Virtual PDF Printer)" de la secció d'impressores locals.

### Confirmació de la impressora virtual

![Confirmació de la impressora virtual PDF](/tasca10/img_t10/captura30.png)

**Anàlisi**: Després de seleccionar CUPS-PDF, el sistema ens mostra la confirmació de la impressora virtual que anem a afegir. La impressora virtual PDF està llista per ser configurada com a impressora de xarxa compartida.

### Configuració bàsica de la impressora

![Configuració bàsica de la impressora](/tasca10/img_t10/captura31.png)

**Anàlisi**: En aquest pas configurem els paràmetres bàsics de la impressora:
- **Nom**: `Virtual_PDF_Printer` (identificador únic)
- **Descripció**: `Virtual PDF Printer` (descripció llegible)
- **Ubicació**: `Server` (on està lògicament ubicada)
- **Connexió**: `cups-pdf:/` (el backend de cups-pdf)
- **Compartició**: Activat - Compartir aquesta impressora

La compartició és essencial perquè els clients de la xarxa puguin detectar i utilitzar aquesta impressora.

### Selecció del fabricant

![Selecció del fabricant](/tasca10/img_t10/captura32.png)

**Anàlisi**: Seleccionem el fabricant de la impressora. Per a la impressora virtual, triem "Generic" de la llista de fabricants. Les opcions inclouen marques comunes com HP, Epson, Ricoh, etc., però per a una impressora virtual, "Generic" és l'opció més adequada.

### Selecció del model específic

![Selecció del model específic](/tasca10/img_t10/captura33.png)

**Anàlisi**: Ara seleccionem el model específic de la impressora virtual. Entre les opcions disponibles per a "Generic", triem:
- **Generic CUPS-PDF Printer (no options) (en)**

Aquest driver és específic per a la impressora virtual PDF i no requereix opcions addicionals de configuració. Altres opcions inclouen impressores genèriques PostScript, PCL, o fins i tot impressores Braille virtuals.

### Confirmació de la creació de la impressora

![Confirmació de la creació de la impressora](/tasca10/img_t10/captura34.png)

**Anàlisi**: La impressora s'ha afegit amb èxit al sistema. El missatge "S'ha afegit amb èxit la impressora Virtual_PDF_Printer" confirma que:
- La impressora està configurada correctament
- El driver s'ha instal·lat
- La impressora està compartida a la xarxa
- Està llesta per rebre treballs d'impressió

## 12. Configuració del client Ubuntu Desktop

### Vista de les impressores al client

![Vista de les impressores al client](/tasca10/img_t10/captura35.png)

**Anàlisi**: A la màquina client (Ubuntu Desktop/Zorin OS), anem a Configuració → Impresores. Veiem que:
- La impressora `Virtual_PDF_Printer_usuari` està detectada
- **Estat**: Preparada
- **Model**: CUPS v1.1
- **Ubicació**: Server
- **Treballs actius**: Cap

El sistema ha detectat automàticament la impressora compartida a través del protocol IPP.

### Procés d'afegir impressora al client

![Procés d'afegir impressora al client](/tasca10/img_t10/captura36.png)

**Anàlisi**: El client està en procés d'afegir la impressora detectada. Veiem dues impressores disponibles:
1. **CUPS-BRF-Printer**: Impressora virtual de text
2. **Virtual_PDF_Printer_usuari**: La nostra impressora virtual PDF compartida

El sistema demana confirmació per afegir la impressora a la llista local del client.

### Confirmació d'addició de la impressora

![Confirmació d'addició de la impressora](/tasca10/img_t10/captura37.png)

**Anàlisi**: El client està a punt per afegir la impressora. Les opcions disponibles són:
- **Cancel·lar**: Interrompre el procés
- **Afegeix impressora**: Confirmar i afegir la impressora

També mostra que la impressora s'identifica amb l'adreça "usuari", que fa referència al servidor on està allotjada.

### Impressora afegida correctament al client

![Impressora afegida correctament al client](/tasca10/img_t10/captura38.png)

**Anàlisi**: La impressora s'ha afegit correctament al client. Ara veiem dues impressores:
1. **CUPS-BRF-Printer**: Detinguda
   - Model: Generic Text-Only Printer
   
2. **Virtual_PDF_Printer_usuari**: Preparada
   - Model: CUPS v1.1
   - Ubicació: Server
   - Treballs actius: Cap

La impressora virtual PDF està ara accessible des del client i llesta per rebre documents.

## 13. Prova de funcionament

### Creació d'un document de prova

![Creació d'un document de prova amb LibreOffice Writer](/tasca10/img_t10/captura39.png)

**Anàlisi**: Aquesta és la captura mostra la creació d'un document amb un petit text.

### Diàleg d'impressió de LibreOffice

![Diàleg d'impressió de LibreOffice](/tasca10/img_t10/captura40.png)

**Anàlisi**: Obrim el diàleg d'impressió a LibreOffice Writer per provar la impressora. Veiem que:
- **Impressora seleccionada**: CUPS-BRF-Printer (per defecte, però podem canviar-ho)
- **Estat**: (normalment mostra "Preparada" per a la impressora virtual)
- **Opcions d'impressió**:
  - Interval i còpies: Totes les pàgines, 1 còpia
  - Compaginació: A4 210mm x 297mm, orientació automàtica
  - Pàgines per full: 1

Ara podem seleccionar "Virtual_PDF_Printer_usuari" i enviar un document de prova.

### Llista d'impressores configurades

![Llista d'impressores al servidor](/tasca10/img_t10/captura41.png)

**Anàlisi**: A la interfície web de CUPS, podem veure totes les impressores configurades al servidor. Veiem dues impressores:

1. **PDF**:
   - Descripció: PDF
   - Model: Generic CUPS-PDF Printer (w/ options)
   - Estat: Inactiva

2. **Virtual_PDF_Printer** (la nostra impressora):
   - Descripció: Virtual PDF Printer
   - Ubicació: Server
   - Model: Generic CUPS-PDF Printer (no options)
   - Estat: Inactiva (però acceptant treballs)

L'estat "inactiva" és normal per a impressores virtuals que no estan físicament connectades, però poden processar treballs.

### Detall de la impressora Virtual_PDF_Printer

![Detall de la impressora Virtual_PDF_Printer](/tasca10/img_t10/captura42.png)

**Anàlisi**: Aquesta pàgina mostra la informació detallada de la nostra impressora virtual:
- **Descripció**: Virtual PDF Printer
- **Ubicació**: Server
- **Controlador**: Generic CUPS-PDF Printer (no options) (color)
- **Connexió**: cups-pdf:/
- **Opcions predeterminades**:
  - Ròtuls: none
  - Paper: iso_a4_210x297mm (A4)
  - Costats: one-sided (una cara)

La impressora està "acceptant treballs" i "compartida", la qual cosa significa que els clients de la xarxa poden enviar-li documents.

### Opcions de manteniment de la impressora

![Opcions de manteniment de la impressora](/tasca10/img_t10/captura43.png)

**Anàlisi**: Aquest menú mostra les opcions de manteniment disponibles per a la impressora:
- **Imprimir pàgina de prova**: Envia una pàgina de prova per verificar el funcionament
- **Limpiar caps d'impressió**: Per a impressores d'injecció (no aplicable aquí)
- **Imprimir pàgina d'auto prova**: Similar a la pàgina de prova
- **Pausar impressora**: Atura temporalment la impressora
- **Rebutjar treballs**: No accepta nous treballs
- **Moure tots els treballs**: Mou els treballs a una altra impressora
- **Cancel·lar tots els treballs**: Elimina tots els treballs pendents

### Enviament de pàgina de prova

![Enviament de pàgina de prova](/tasca10/img_t10/captura44.png)

**Anàlisi**: Quan seleccionem "Imprimir pàgina de prova", el sistema confirma que s'ha enviat la pàgina. El missatge indica:
- "Pàgina de prova enviada"
- "El número del treball és Virtual_PDF_Printer-4"

Això significa que s'ha creat un nou treball d'impressió amb ID 4 a la cua de la impressora Virtual_PDF_Printer.

### Llista de treballs completats

![Llista de treballs completats](/tasca10/img_t10/captura45.png)

**Anàlisi**: A la secció de treballs de la impressora, podem veure tots els documents que s'han processat:
- **Virtual_PDF_Printer-4**: Completat el 17 dic 2025 17:40:12
- **Virtual_PDF_Printer-3**: Completat el 17 dic 2025 17:39:54
- **Virtual_PDF_Printer-2**: Completat el 17 dic 17:39:22
- **Virtual_PDF_Printer-1**: Completat el 17 dic 17:38:02

Cada treball mostra:
- **ID**: Identificador únic
- **Nom**: Desconegut (el sistema no pot determinar el nom del document)
- **Usuari**: Retingut (per privadesa)
- **Mida**: 1k (aproximadament 1 KB cada document)
- **Pàgines**: 1
- **Estat**: Completat
- **Control**: Opció per reimprimir el treball

## 15. Verificació dels PDF generats al servidor

### Llista de directoris de l'usuari

```bash
ls -al
```

![Llista de directoris de l'usuari](/tasca10/img_t10/captura46.png)

**Anàlisi**: Llistem el contingut del directori home de l'usuari. Veiem que hi ha un nou directori anomenat `PDF`:
- **Nom**: PDF
- **Permisos**: drwx--- (directori, propietari té lectura, escriptura, execució)
- **Propietari**: usuari
- **Grup**: usuari
- **Mida**: 4096 bytes
- **Data de modificació**: 17 dic 17:40

Aquest directori ha estat creat automàticament per cups-pdf per emmagatzemar els documents PDF generats.

### Accés al directori PDF

```bash
cd PDF
```

![Accés al directori PDF](/tasca10/img_t10/captura47.png)

**Anàlisi**: Ens movem al directori `PDF` utilitzant la comanda `cd PDF`. El prompt canvia de `usuari@usuari:~$` a `usuari@usuari:~/PDF$`, indicant que ara estem dins del directori PDF.

### Llista dels PDF generats

```bash
ls -l
```

![Llista dels PDF generats](/tasca10/img_t10/captura48.png)

**Anàlisi**: Llistem el contingut del directori PDF. Veiem dos fitxers PDF generats:
1. **Test_Page.pdf**:
   - Mida: 129,536 bytes (aproximadament 129 KB)
   - Data: 17 dic 17:40
   - Propietari: usuari
   - Grup: usuari

2. **Test_Page___PDF.pdf**:
   - Mida: 129,623 bytes (aproximadament 130 KB)
   - Data: 17 dic 17:38
   - Propietari: usuari
   - Grup: usuari

Aquests són els documents que s'han "imprès" a través de la impressora virtual. El sistema ha convertit les pàgines de prova en fitxers PDF i els ha guardat en aquest directori.

## **Conclusió:**

Hem implementat amb èxit un servidor d'impressió centralitzat utilitzant CUPS a Ubuntu Server. La impressora virtual PDF funciona correctament, convertint documents enviats des del client (o des del propi servidor) en fitxers PDF al servidor. Aquesta configuració demostra:

1. **Viabilitat tècnica**: Un sistema d'impressió centralitzat és possible amb software lliure
2. **Estalvi de costos**: No es necessiten impressores físiques per a proves
3. **Simplificació de gestió**: Una única impressora virtual pot servir a múltiples clients
4. **Documents digitals**: Els documents es poden arxivar digitalment en format PDF

---

[Explicació de la tasca](README.md)

[Tornar a la pàgina principal](../)
