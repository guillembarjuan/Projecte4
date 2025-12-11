# T06: Accés remot. Escriptori remot (RDP) - Guia d'Configuració i Ús

## Introducció
Aquesta guia documenta el procés per configurar i utilitzar l'Escriptori Remot (RDP) entre un client Windows 11 i un servidor Zorin OS (Linux). L'objectiu és establir una connexió remota gràfica que permeti prendre el control complet d'un equip per a tasques de suport tècnic. Es detallen tant la configuració del servidor (equip remot) com la del client (des d'on es connecta l'usuari).

## Fase 1: Configuració de l'Equip Windows 11 (Servidor RDP)

### Pas 1: Habilitar l'Escriptori Remot a Windows
Primer, cal anar a Configuració > Sistema > Escriptori remot i activar la funció.

![Configuració inicial d'Escriptori remot desactivada](/tasca06/img_t06/captura1.png)

En aquesta pantalla es mostra l'estat inicial. L'Escriptori remot està **Desactivat**. Per habilitar-lo, caldrà prémer la casella de verificació.

![Confirmació per habilitar l'Escriptori remot](/tasca06/img_t06/captura2.png)

En intentar activar l'opció, Windows demana confirmació perquè l'habilitació d'aquesta funció pot representar un risc de seguretat si no es configura correctament. Es mostren els avantatges: "Tu y los usuarios seleccionados [...] podréis conectaros remotamente a este equipo". S'accepta per continuar.

### Pas 2: Configuració de l'Escriptori Remot Actiu
Un cop habilitat, es pot veure la configuració principal.

![Configuració d'Escriptori remot activada](/tasca06/img_t06/captura3.png)

Els elements principals configurats són:
1.  **Escriptori remot:** Activat.
2.  **Nom de PC:** `DESKTOP-T07NR01`. Aquest és el nom que els clients utilitzaran per connectar-s'hi.
3.  **Usuaris de Escriptori remot:** S'ha de configurar quins usuaris poden connectar-se remotament. Per defecte, els membres del grup Administradors tenen accés.

### Pas 3: Gestionar Usuaris amb Accés Remot
Es prem l'opció "Seleccionar quién puede tener acceso remoto a este equipo".

![Gestió d'usuaris d'Escriptori remot](/tasca06/img_t06/captura8.png)

En aquesta finestra es veu que l'usuari `client` ja té accés. Es poden afegir o eliminar usuaris. És crucial afegir aquí només els comptes d'usuari que necessitin accés remot per motius de seguretat.

Per afegir un usuari, es prem "Agregar...".

![Selecció d'usuaris per afegir-los a la llista d'accés remot](/tasca06/img_t06/captura9.png)

S'obre un diàleg on es poden escriure els noms d'usuaris (objectes) que s'afegiran a la llista de permisos. En aquest cas, s'ha escrit `prova`. Es prem "Aceptar" per confirmar.

### Pas 4: Crear un Nou Usuari Local (Opcional)
Si cal crear un usuari específic per a connexions remotes, es pot fer des de Configuració > Cuentas > Otros usuarios.

![Pantalla per afegir un altre usuari](/tasca06/img_t06/captura4.png)

Es prem "Agregar otro usuario" o "Agregar cuenta" per iniciar l'assistent de creació.

![Creació d'un usuari per a l'equip](/tasca06/img_t06/captura5.png)
![Creació d'un usuari per a l'equip - Pas 2](/tasca06/img_t06/captura6.png)

L'assistent guia a l'usuari per crear un compte local. És important:
*   Escollir un **nom d'usuari** clar (ex: `prova1`).
*   Assignar-li una **contrasenya forta** però recordable.
*   Confirmar que és un compte local (no necessàriament un compte de Microsoft).

![Llista d'usuaris amb el nou compte creat](/tasca06/img_t06/captura7.png)

Després de la creació, el nou usuari `prova1` apareix a la llista d'"Otros usuarios". Ara també podria ser afegit a la llista d'usuaris d'Escriptori Remot seguint el **Pas 3**.

## Fase 2: Configuració de l'Equip Zorin OS (Client RDP)

### Pas 1: Preparar el Client Remmina (Linux)
A Zorin OS, l'aplicació per a connexions RDP es diu **Remmina**. Es pot buscar i obrir des del menú d'aplicacions.

![Cercant l'aplicació Remmina al menú d'inici de Zorin](/tasca06/img_t06/captura20.png)

Un cop oberta, la interfície de Remmina és relativament senzilla.

![Interfície principal de Remmina sense connexions](/tasca06/img_t06/captura11.png)

Aquesta és la finestra principal on es mostraran totes les connexions RDP guardades. Inicialment està buida ("Total de 0 elementos").

### Pas 2: Crear una Nova Connexió RDP
Per crear una nova connexió, es prem el botó corresponent (normalment un símbol +) i es selecciona el protocol RDP.

![Finestra per crear una nova connexió RDP a Remmina](/tasca06/img_t06/captura12.png)

S'han d'omplir els camps següents:
*   **Nom:** Un nom descriptiu per a la connexió (ex: `DESKTOP-T07NR01.local`).
*   **Servidor:** L'adreça o nom de l'equip remot. En aquest cas, el nom de l'equip Windows: `DESKTOP-T07NR01.local`.
*   **Nom d'usuari:** L'usuari creat o autoritzat a Windows (`prova1`).

### Pas 3: Establir la Connexió
Després de configurar la connexió, es prem per connectar.

![Finestra de diàleg per introduir les credencials](/tasca06/img_t06/captura13.png)
![Finestra de diàleg per introduir les credencials - Detall](/tasca06/img_t06/captura14.png)

Remmina demanarà les credencials (contrasenya) per a l'usuari especificat. Es poden introduir manualment o, si es confia en l'equip client, marcar "Guardar contraseña" per a futures connexions.

## Fase 3: Establiment de la Connexió i Comportament

### Pas 1: Sol·licitud de Connexió a l'Equip Remot
Quan el client Linux intenta connectar-se, l'equip Windows ho detecta i mostra una alerta.

![Alerta a l'equip Windows per una connexió remota entrant](/tasca06/img_t06/captura16.png)

Aquesta alerta és crítica per a la seguretat. Informa a l'usuari local de l'equip Windows que algú (`prova1` des de `DESKTOP-T07NR01.local`) vol connectar-se. L'usuari local té dues opcions:
*   **Aceptar:** Permet la connexió, que provocarà que la sessió local actual es tanqui (es "desconnecti") per donar pas a la sessió remota.
*   **Cancelar:** Denega la connexió.

Si l'usuari local no fa res, la connexió es denegarà automàticament després de 30 segons.

### Pas 2: Connexió Exitosa (o No)
Si la connexió es denega o l'usuari ja té una sessió oberta, el client pot rebre un missatge d'error.

![Missatge d'error per sessió ja iniciada](/tasca06/img_t06/captura15.png)

Aquest missatge indica que un altre usuari (`prova1`) ja ha iniciat sessió a l'equip remot. Si es continua, aquesta sessió existent es desconnectarà (possiblement causant pèrdua de treball no guardat). És una protecció per evitar múltiples sessions gràfiques simultànies en edicions no servidor de Windows.

Si la connexió és exitosa i l'usuari local ha acceptat, el client de Remmina mostrarà l'escriptori remot de Windows.

![Escriptori remot de Windows vist des del client Zorin](/tasca06/img_t06/captura17.png)

En aquest punt, l'usuari del client Linux té control complet sobre el ratolí, teclat i pantalla de l'equip Windows, com si estigués físicament davant seu.

## Fase 4: Connexió des d'un Client Windows a un Servidor Zorin

El propi Windows inclou un client RDP (anomenat "Conexión a Escritorio remoto"). També es pot utilitzar per connectar-se a altres equips Windows o Linux que suportin RDP.

![Cercant l'aplicació 'Conexión a Escritorio remoto' a Windows](/tasca06/img_t06/captura20.png)


## Fase 4: Connexió des d'un Client Windows a un Servidor Zorin

### Pas 1: Configurar el Servidor RDP a Zorin OS
Abans de connectar-se des de Windows, cal assegurar-se que el Zorin OS té el servidor d'Escriptori Remot (VNC/RDP) activat.

![Configuració del servidor d'escriptori remot a Zorin OS](/tasca06/img_t06/captura18.png)

A la configuració de Zorin, s'ha d'anar a la secció "Escritorio remoto". Aquí es pot veure que l'estat actual és "Apagado". Per habilitar-ho, cal anar a l'aplicació específica de configuració.

![Configuració detallada del servidor remot a Zorin OS](/tasca06/img_t06/captura19.png)

En aquesta pantalla es mostren les opcions principals:
1. **Escritorio remoto:** Per activar o desactivar les connexions
2. **Activar protocolo VNC heredado:** És el protocol compatible amb clients RDP de Windows
3. **Control remoto:** Permet que el client controli la pantalla del servidor
4. **Nombre del dispositivo:** `client-VirtualBox` (identificador per a la connexió)
5. **Dirección del escritorio remoto:** `ms-rdp://client-VirtualBox.local` (format per a clients moderns)
6. **Credenciales:** Nom d'usuari (`client`) i contrasenya per accedir

### Pas 2: Configurar la Connexió des del Client RDP de Windows
Des de Windows 11, s'utilitza l'aplicació "Conexión a Escritorio remoto" per establir la connexió.

![Cercant l'aplicació 'Conexión a Escritorio remoto' a Windows](/tasca06/img_t06/captura20.png)

Es pot trobar l'aplicació mitjançant la cerca del sistema. Un cop oberta, es procedeix a configurar la connexió.

![Configuració de la connexió en el client RDP de Windows](/tasca06/img_t06/captura21.png)

En aquesta finestra es configuren els paràmetres bàsics:
*   **Equipo:** S'ha d'indicar el nom de l'equip remot Zorin, que en aquest cas és `client-VirtualBox.local`.
*   **Usuari:** S'ha d'escriure el nom d'usuari vàlid al Zorin, aquí `client`.
*   **Credenciales:** Es marca l'opció "Se solicitarán credenciales al conectarse" perquè demani la contrasenya en connectar.

### Pas 3: Introduir les Credencials
En prémer "Conectar", es demanarà la contrasenya de l'usuari remot.

![Diàleg per introduir la contrasenya de l'usuari remot](/tasca06/img_t06/captura22.png)

Aquesta finestra sol·licita la contrasenya per a l'usuari `client` a l'equip `client-VirtualBox.local`. És important introduir-la correctament per a l'autenticació. Es pot marcar "Recordar cuenta" per agilitzar connexions futures, però només en entorns de confiança.

### Pas 4: Gestionar l'Alerta de Seguretat del Certificat
En connectar-se a un servidor Linux amb certificats autosignats, el client Windows mostrarà una alerta de seguretat.

![Alerta de seguretat per certificat no fiable](/tasca06/img_t06/captura23.png)

Aquesta alerta és normal en connexions a equips no de domini o amb certificats autosignats. Informa de dos problemes:
1.  No es pot comprovar la identitat de l'equip remot.
2.  El certificat (amb nom `GNOME`) no ve d'una entitat de confiança.

**Decisió crítica:**
*   **Si:** S'ha de prémer aquest botó només si es confia en la màquina de destí (Zorin) i la connexió es realitza dins d'una xarxa segura (com una LAN interna o VPN). No és recomanable en xarxes públiques.
*   **No:** Es cancel·la la connexió. És l'opció més segura si hi ha qualsevol dubte.
*   **No volver a preguntarme...:** Es pot marcar aquesta opció un cop es confia en l'equip remot per evitar l'alerta en connexions futures.

### Pas 5: Connexió Exitosa
Un cop superada l'alerta de seguretat (si s'ha escollit "Si"), l'escriptori gràfic del Zorin OS es mostrarà en una finestra al client Windows.

![Escriptori remot del Zorin OS vist des del client Windows](/tasca06/img_t06/captura24.png)

En aquesta captura es pot veure l'escriptori del Zorin OS (`client-VirtualBox`) amb el seu fons de pantalla per defecte i alguns accessos directes. El client Windows ara té control complet sobre el servidor Zorin, permetent realitzar tasques d'administració o suport tècnic com si estigués físicament davant de l'equip remot.

## Conclusió General i Bones Pràctiques

Aquesta guia ha cobrit el flux complet de connexions RDP bidireccionals:

1.  **De Linux (Zorin/Remmina) a Windows:**
    *   Configurar Windows com a servidor (habilitar RDP, gestionar usuaris).
    *   Utilitzar Remmina al client Linux per connectar-se.
    *   Acceptar la connexió des del costat del servidor Windows.

2.  **De Windows a Linux (Zorin):**
    *   Configurar Zorin com a servidor (habilitar "VNC heredado").
    *   Utilitzar el client RDP de Windows per connectar-se.
    *   Gestionar l'alerta de certificat autosignat amb precaució.

---

[Explicació de la tasca](README.md)

[Tornar a la pàgina principal](../)
