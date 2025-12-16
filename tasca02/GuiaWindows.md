# GuiaWindows.md – Còpies de seguretat amb Duplicati (Windows)

En aquesta guia es mostra pas a pas com configurar còpies de seguretat a Windows 11 amb Duplicati per complir l’esquema 3‑2‑1: una còpia al disc secundari de 10 GB de la màquina virtual i una còpia al núvol (Google Drive).

> **Abans de començar:** a VirtualBox s’ha creat una màquina virtual amb Windows 11 i un segon disc dur virtual de 10 GB, que servirà per emmagatzemar les còpies de seguretat.

***

## Instal·lació de Duplicati a Windows

Per començar, des del navegador de la màquina virtual busquem “Duplicati” per accedir a la pàgina oficial del projecte i descarregar el programa.

![Navegador de Windows buscant “Duplicati” per accedir a la web oficial](/tasca02/img_t02_win/captura1.png)

Accedim al web oficial i a la secció de descàrregues seleccionem la versió corresponent a Windows 64 bits fent clic al botó **Download for Windows**.

![Pàgina oficial de Duplicati amb les opcions de descàrrega per a Windows.](/tasca02/img_t02_win/captura2.png)

Quan es completa la descàrrega, a la part superior dreta del navegador apareix la notificació amb el fitxer MSI; des d’allà l’obrim per començar la instal·lació.

![Descàrrega del fitxer d’instal·lació de Duplicati des del navegador.](/tasca02/img_t02_win/captura3.png)

S’obre l’assistent d’instal·lació de Duplicati. A la pantalla de benvinguda fem clic a **Next** per continuar amb el procés.

![Inici de l’assistent d’instal·lació de Duplicati a Windows.](/tasca02/img_t02_win/captura4.png)

Un cop revisats els paràmetres, la pantalla “Ready to install Duplicati” indica que tot és preparat; premem **Install** per instal·lar el programa al sistema.

![Pantalla “Ready to install” abans de començar la instal·lació.](/tasca02/img_t02_win/captura6.png)

Quan la instal·lació finalitza correctament, l’assistent mostra el missatge “Completed the Duplicati Setup Wizard”. Premem **Finish** per tancar-lo i l’aplicació queda disponible.

![Missatge final de l’assistent indicant que Duplicati s’ha instal·lat correctament.](/tasca02/img_t02_win/captura5.png)

En obrir Duplicati per primer cop, es mostra la interfície web local al navegador, on encara no hi ha cap tasca programada ni cap còpia definida.

![Interfície inicial de Duplicati sense cap còpia de seguretat creada.](/tasca02/img_t02_win/captura7.png)

***

## Creació del backup local al disc secundari

Per crear una nova còpia de seguretat, fem clic a **Add backup** i triem **Add a new backup**, que permet definir un pla de còpia des de zero.

![Pantalla per afegir una nova còpia de seguretat (“Add a new backup”).](/tasca02/img_t02_win/captura8.png)

A l’apartat **General** definim el nom de la còpia (per exemple, `backup tasca2`) i, per protegir les dades, activem el xifratge amb **AES‑256** i establim una contrasenya robusta per al backup.

![Configuració general del backup backup tasca2 amb xifratge activat.](/tasca02/img_t02_win/captura9.png)

A continuació, a **Backup destination** seleccionem **File system** com a destí, ja que aquesta primera còpia s’emmagatzemarà al disc secundari de 10 GB de la màquina virtual.

![Selecció del tipus de destí “File system” per a la còpia local.](/tasca02/img_t02_win/captura10.png)

Al navegar pel sistema de fitxers, seleccionem el nou volum (per exemple, unitat **E:**) com a carpeta on Duplicati guardarà els arxius de còpia; si cal, es pot crear una carpeta específica dins d’aquest disc.

![Selecció del disc secundari de 10 GB com a carpeta de destí del backup.](/tasca02/img_t02_win/captura11.png)

En l’apartat **Source Data** de la creació del backup es tria com a dades d’origen la carpeta **Home** de l’usuari de Windows, de manera que es protegeixen Documents, Escriptori i la resta de perfils.

![Selecció de la carpeta Home de l’usuari com a origen de les dades.](/tasca02/img_t02_win/captura12.png)

Després es defineix la **planificació**: en aquest cas es configura el backup perquè s’executi automàticament **cada hora**, marcant tots els dies de la setmana perquè la protecció sigui contínua.

![Planificació del backup local perquè s’executi automàticament cada hora.](/tasca02/img_t02_win/captura13.png)

A les **Opcions** finals, es pot ajustar la mida dels volums remots (per exemple, 50 MB per fitxer) i la política de retenció; inicialment es manté “Conservar totes les còpies” per poder fer proves d’històric.

![Opcions avançades del backup (mida dels volums, política de retenció, etc.).](/tasca02/img_t02_win/captura14.png)

Quan es guarda la configuració, a la pantalla principal apareix el nou backup `backup tasca2`; en aquest punt ja s’ha fet clic al botó **Start** i es mostra que la primera còpia local s’ha completat amb èxit, indicant durada, mida d’origen i destí, i la propera execució programada.

![Vista principal preparada per crear un nou backup addicional (el del núvol).](/tasca02/img_t02_win/captura15.png)

![Pantalla principal mostrant el backup local ja executat amb la primera còpia feta.](/tasca02/img_t02_win/captura16.png)

***

## Creació del backup al núvol (Google Drive)

Per complir l’esquema 3‑2‑1 també es crea una còpia remota a Google Drive. Des de **Add backup** es defineix un nou pla de còpia per al núvol.

A l’apartat **Backup destination** d’aquest nou backup es tria **Google Drive** com a destí; aquesta opció permet que Duplicati es connecti al compte de Google mitjançant OAuth.

![Configuració del backup al núvol seleccionant Google Drive com a destí.](/tasca02/img_t02_win/captura17.png)

Un cop autoritzat l’accés, es defineix la **ruta de la carpeta** dins de Google Drive on s’emmagatzemaran les còpies (per exemple, `carpeta_backup`) i a sota s’hi mostra el **codi d’autorització** `AuthID` associat al compte.

![Destí de Google Drive configurat amb la carpeta carpeta_backup i l’AuthID.](/tasca02/img_t02_win/captura18.png)

Per a les dades d’origen d’aquest backup al núvol es torna a seleccionar la carpeta **Home** de l’usuari, igual que en el backup local, de manera que ambdós plans protegeixen el mateix conjunt de fitxers.

![Selecció de la carpeta Home com a origen per al backup de Google Drive.](/tasca02/img_t02_win/captura19.png)

En la planificació d’aquest segon backup es configura l’execució automàtica a les **18:00** cada dia, complint el requisit de fer una còpia diària al núvol a aquesta hora concreta.

![Planificació del backup de Google Drive perquè s’executi cada dia a les 18:00.](/tasca02/img_t02_win/captura20.png)

Després de guardar la configuració i executar el backup de prova, a la pantalla principal apareixen dos plans: el backup local `backup tasca2` i el backup `backup drive` cap a Google Drive, amb informació de la darrera còpia, mida i pròxima execució.

![Vista principal amb els dos backups definits: local i Google Drive.](/tasca02/img_t02_win/captura21.png)

***

## Prova de restauració des del disc secundari

Per verificar que les còpies funcionen, primer es creen fitxers de prova (per exemple `prova.txt`, `hola.txt`, `fortnite.txt`) dins de la carpeta **Documents** de l’usuari, que queden inclosos en el proper backup.

![Carpeta Documents amb diversos fitxers de prova creats per al backup local.](/tasca02/img_t02_win/captura23.png)

Després d’haver generat i programat diverses còpies, a la pantalla principal de Duplicati es veu que el backup local disposa de múltiples **versions**, cosa que permet tornar enrere a diferents punts en el temps.

![Vista de Duplicati mostrant diverses versions del backup local disponibles.](/tasca02/img_t02_win/captura22.png)

Per simular una pèrdua de dades, s’esborren tots els fitxers de la carpeta **Documents**, de manera que el directori queda completament buit abans de començar la restauració.

![Carpeta Documents buida després d’esborrar els fitxers per provar la restauració.](/tasca02/img_t02_win/captura24.png)

A Duplicati, des del menú de tres punts del backup local, es tria **Restore**; a la pantalla de selecció de fitxers es tria una versió concreta (per exemple la de les 15:05) i es marquen els fitxers que es volen recuperar.

![Pantalla principal de Duplicati](/tasca02/img_t02_win/captura25.png)

![Opcions quan es prem el boto dels tres punts](/tasca02/img_t02_win/captura26.png)

![Pantalla de Duplicati per seleccionar la versió i els fitxers a restaurar del backup local.](/tasca02/img_t02_win/captura27.png)

En les **opcions de restauració** s'ha d'indicar el disc de 10 GB que hem afegit, és on aniran tots els arxius restaurats. (A la captura esta seleccionada la opció de "original location" però esta malament)

![Opcions de restauració indicant que es recuperin els fitxers a la ubicació original.](/tasca02/img_t02_win/captura28.png)

Un cop iniciat el procés, la pantalla de **progress** mostra l’estat de la restauració i, quan acaba sense errors, apareix el missatge **Restore completed** confirmant que les dades s’han recuperat correctament.

![Pantalla de progrés de restauració amb el missatge Restore completed.](/tasca02/img_t02_win/captura29.png)

Finalment, en tornar a obrir la carpeta **Nuevo Vol** de Windows, es comprova que els fitxers de prova (`prova`, `hola`, `fortnite`) han estat restaurats.

![Carpeta nuevo vol mostrant els fitxers de prova restaurats des del disc secundari.](/tasca02/img_t02_win/captura30.png)

***

## Restauració des de Google Drive

A la carpeta **Documents** es creen dos fitxers de prova, `provadrive1` i `provadrive2`.  
Aquests arxius serviran per comprovar que la còpia de seguretat a Google Drive inclou correctament els fitxers nous que es generen a l’equip.

![Carpeta Documents amb els fitxers provadrive1 i provadrive2 creats per provar Drive.](/tasca02/img_t02_win/captura31.png)

Després de crear els fitxers, es deixa que s’executi el backup `backup drive` o bé s’inicia manualment.  
En aquesta captura es veu la barra de progrés del pla de còpia de Google Drive, indicant el nombre de fitxers i els GB que queden per pujar al núvol.

![Vista principal de Duplicati amb més versions creades al backup local i al de Drive.](/tasca02/img_t02_win/captura32.png)

![Barra de progrés del backup backup drive pujant dades a Google Drive.](/tasca02/img_t02_win/captura33.png)

Quan el backup al núvol ha finalitzat amb èxit, Duplicati mostra a la pantalla principal que el pla `backup drive` té una nova versió creada.  

![Pla backup drive finalitzat amb èxit i opció de Restore disponible.](/tasca02/img_t02_win/captura34.png)

Abans de restaurar, s’esborren els fitxers `provadrive1` i `provadrive2` de la carpeta **Documents**, que queda completament buida.  
Això simula una pèrdua de dades i permet comprovar que la posterior restauració es fa exclusivament a partir de la còpia de Google Drive.

![Carpeta Documents novament buida després d’esborrar els fitxers de prova de Drive.](/tasca02/img_t02_win/captura35.png)

De tornada a Duplicati, es comprova que tant el backup local `backup tasca2` com el backup `backup drive` disposen de diverses versions.  
Des del menú de tres punts d’aquest pla apareixen les opcions **Restore** i **Export**, que permeten iniciar el procés de restauració o exportar la configuració.
En aquest punt, des del pla `backup drive` s’escollirà l’opció **Restore** per recuperar els fitxers eliminats.

![Vista principal de Duplicati mostrant versions actualitzades dels dos plans de còpia.](/tasca02/img_t02_win/captura36.png)

En iniciar la restauració, l’assistent mostra la pantalla **Select files** amb la llista de versions disponibles del backup de Google Drive.  
S’escull la versió on ja existien els fitxers de prova i, a l’arbre de carpetes, es marquen `provadrive1.txt` i `provadrive2.txt` per restaurar-los.

![Pantalla de selecció de versió i marcatge dels fitxers provadrive1.txt i provadrive2.txt.](/tasca02/img_t02_win/captura37.png)

A continuació es passa a la pantalla **Options** de la restauració.  
Aquí es deixa seleccionada l’opció **Pick location**, perquè els fitxers es recuperin en el disc secundari.

![Opcions de restauració des de Drive mantenint la restauració a la carpeta original.](/tasca02/img_t02_win/captura38.png)

Quan s’inicia el procés, Duplicati mostra la pantalla de progrés de la restauració amb el missatge **Restore completed** quan acaba sense errors.  
A la secció de **Logs** es poden veure tots els passos que ha realitzat l’eina: connexió a Google Drive, lectura de la còpia i escriptura dels fitxers restaurats.

![Pantalla de progrés de la restauració des de Google Drive amb Restore completed i logs.](/tasca02/img_t02_win/captura39.png)

Després de la restauració, s’obre l'explorador d'arxius i es comprova que els fitxers `provadrive1` i `provadrive2` han tornat a aparèixer.  
Això demostra que la recuperació des de la còpia de Google Drive ha estat correcta i que és possible restaurar dades perdudes utilitzant el backup remot.

![Carpeta nuvo vol amb provadrive1 i provadrive2 restaurats des de Google Drive.](/tasca02/img_t02_win/captura40.png)

***

## Conculsió

Aquesta guia de Windows demostra com, amb una eina gratuïta com Duplicati, es pot desplegar fàcilment una política de còpies de seguretat 3‑2‑1 en un entorn domèstic o d’oficina. S’ha configurat una còpia horària al disc secundari i una còpia diària a Google Drive, totes dues xifrades i versionades, i s’ha comprovat mitjançant diverses proves de restauració que és possible recuperar tant fitxers concrets com carpetes senceres en cas d’error o pèrdua de dades.

---

[Guia de linux](GuiaServer.md)

[Explicació de la tasca](README.md)

[Tornar a la pàgina principal](../)

