# 📁 T09: Servidor de Fitxers Linux – Implementació NFS (Tasca Individual)

## 📄 Breu descripció

## 🔧 Introducció
En aquesta tasca ens hem trobat amb un dels requisits més habituals que ens demanen els clients que treballen en entorns Linux: **centralitzar les dades** per evitar la dispersió d’informació i els problemes associats.

La nostra missió ha estat donar resposta a la necessitat d’un client realista dins del projecte EverPia, implementant una solució funcional basada en **NFS (Network File System)**.

## 🏢 El cas client: *DevOptimize Solutions*
El nostre client és *DevOptimize Solutions*, una startup de desenvolupament de programari que treballa exclusivament amb Linux. Actualment tenen un problema greu:

- Cada desenvolupador guarda còpies locals del codi.
- No hi ha cap control de versions intern.
- La informació es duplica.
- Les errades per desincronització són constants.
- La productivitat està caient en picat.

Ens han contractat per desplegar un **servidor de fitxers centralitzat** que permeti:

- Unificar arxius.
- Evitar conflictes i desordres.
- Millorar el flux de treball intern.

Com que tot l’entorn és Linux, la millor opció és NFS, la solució nativa, ràpida i eficient per compartir directoris en xarxa.

## 🎯 Objectiu de la demostració
El client treballa **sense autenticació centralitzada** (sense LDAP) i no té plans d’implementar-la a curt termini. Per aquest motiu, ens han demanat una **prova de concepte** que els permeti veure:

- Com quedarà la solució final.
- Quines són les possibilitats reals del sistema.
- Quines limitacions tenen sense un entorn d’identitats centralitzat.

Per això, en aquesta tasca hem:

- Creat un **servidor NFSv3**.
- Configurat un **client Linux** que consumeix els recursos compartits.
- Creat usuaris i grups per simular l’escenari del client.
- Gestionat permisos i propietats amb `chmod` i `chown`.
- Ajustat les exportacions del servidor mitjançant **/etc/exports**.

La finalitat és demostrar el control d’accés real que permet NFS en un entorn com el del client.

## 🗂️ Repositori de la tasca
En aquest repositori es troba la descripció completa de les activitats a realitzar:  
🔗 https://github.com/SMX2n/Projecte04-NFS

## 📚 Materials i recursos utilitzats
- Material propi: *UD5. AA1. NFS* — Moodle de Sistemes Operatius en Xarxa.  
- Ruiz, P. (2021).  
  - *NFS (parte 1): Instalación en un servidor Ubuntu 20.04 LTS*.  
  - *NFS (parte 2): Instalación en un cliente Ubuntu 20.04 LTS*.  
- Ubuntu Server Documentation — *Network File System (NFS)*.

---

Aquesta tasca ens ha permès entendre, implementar i demostrar una solució NFS professional, simulant una situació real d’empresa i treballant permisos, usuaris, grups i comparticions tal com ho faríem en un entorn productiu.

---

[Guia de la tasca](guia.md)

---

[Tornar a la pàgina principal](../)
