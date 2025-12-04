# T05: Accés Remot. Connexió via SSH - Guia d'Implementació

## Introducció
Aquesta guia documenta el procés complet de configuració i ús de SSH (Secure Shell) per a l'accés remot segur als sistemes de la consultora. SSH és l'estàndard de la indústria per a l'administració remota de servidors, proporcionant una connexió xifrada i segura tant per a sistemes Linux com Windows.

## Fase 1: Configuració del Servidor SSH a Ubuntu

### Actualització del sistema
Abans de començar, és essencial assegurar que el sistema estigui actualitzat:

```bash
sudo apt update && sudo apt upgrade -y
```

![Actualització del sistema Ubuntu](/tasca05/img_t05/captura1.png)

Aquesta comanda actualitza la llista de paquets i aplica totes les actualitzacions disponibles, garantint que el sistema tingui les darreres correccions de seguretat.

### Instal·lació del servidor SSH

```bash
sudo apt install ssh -y
```

![Instal·lació del paquet SSH](/tasca05/img_t05/captura3.png)

El paquet SSH (OpenSSH) s'instal·la al sistema, proporcionant tant el client com el servidor per a connexions segures.

### Configuració del servei SSH

```bash
sudo systemctl enable ssh
sudo systemctl start sshd
sudo systemctl status sshd
```

![Habilitació i inici del servei SSH](/tasca05/img_t05/captura4.png)
![Estat del servei SSH actiu](/tasca05/img_t05/captura9.png)

El servei SSH s'habilita per a l'inici automàtic, s'inicia manualment i es verifica el seu estat. La sortida mostra que el servidor està escoltant en el port 22 de totes les interfícies de xarxa.

### Configuració de seguretat SSH

```bash
sudo nano /etc/ssh/sshd_config
```

![Edició del fitxer de configuració SSH](/tasca05/img_t05/captura5.png)

S'obre l'editor nano per modificar el fitxer de configuració principal del servidor SSH, on es poden ajustar paràmetres de seguretat com els usuaris permesos i l'accés root.

```bash
AllowUsers usuari
```

![Configuració AllowUsers al sshd_config](/tasca05/img_t05/captura6.png)

Aquesta línia s'afegeix al fitxer de configuració per restringir l'accés remot només a l'usuari especificat, millorant la seguretat del sistema.

### Reinici del servei SSH

```bash
sudo systemctl restart sshd
```

![Reinici del servei SSH](/tasca05/img_t05/captura8.png)

Després de modificar la configuració, és necessari reiniciar el servei perquè els canvis tinguin efecte.

### Verificació de la configuració

```bash
sudo sshd
```

![Verificació de la configuració SSH](/tasca05/img_t05/captura7.png)

Aquesta comanda verifica la sintaxi del fitxer de configuració sense iniciar realment el dimoni del servidor, útil per detectar errors abans d'aplicar els canvis.

## Fase 2: Configuració del Client Windows 11

###Creació de la maquina

![Informació del sistema Windows](/tasca05/img_t05/captura10.png)

Aquesta captura mostra la maquina de windows 11 creada.

### Accés al Panell de Control

![Cerca del Panell de Control](/tasca05/img_t05/captura11.png)
![Panell de Control de Windows 11](/tasca05/img_t05/captura12.png)

El Panell de Control s'obre per configurar les opcions de xarxa i proxy del sistema Windows.

### Configuració de xarxa

![Secció de xarxes al Panell de Control](/tasca05/img_t05/captura13.png)

Des de la secció "Xarxes i Internet" del Panell de Control, es poden configurar les connexions de xarxa i els paràmetres de proxy.

### Configuració del proxy SOCKS

![Propietats d'Internet - Configuració de connexió](/tasca05/img_t05/captura14.png)
![Configuració manual del proxy](/tasca05/img_t05/captura15.png)
![Configuració avançada del proxy SOCKS](/tasca05/img_t05/captura16.png)
![Configuració final del proxy](/tasca05/img_t05/captura17.png)

Per configurar el túnel SSH com a proxy SOCKS, s'accedeix a les propietats d'Internet → Connexions → Configuració de LAN. S'habilita l'ús d'un servidor proxy amb adreça 127.0.0.1 i port 9876, especificant que és de tipus SOCKS. Aquesta configuració permet que tot el trànsit del navegador passi pel túnel SSH.

## Fase 3: Instal·lació d'eines de diagnòstic

### Descarrega de Wireshark

![Pàgina de descàrrega de Wireshark](/tasca05/img_t05/captura18.png)

Wireshark es descarrega des del lloc web oficial per analitzar el trànsit de xarxa i verificar el funcionament del túnel SSH.

### Instal·lació de Wireshark

![Assistent d'instal·lació de Wireshark](/tasca05/img_t05/captura19.png)

L'assistent d'instal·lació de Wireshark guia l'usuari a través del procés d'instal·lació del analitzador de paquets.

### Instal·lació de Npcap

![Instal·lació completada de Npcap](/tasca05/img_t05/captura20.png)

Npcap és una biblioteca necessària per a la captura de paquets a Windows. La instal·lació es completa satisfactòriament, permetent a Wireshark accedir a les interfícies de xarxa.



---

[Explicació de la tasca](README.md)

[Tornar a la pàgina principal](../)
