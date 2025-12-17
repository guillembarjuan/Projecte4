# 🖨️ T10: Servidor d’Impressió Linux — CUPS (PoC)

## 📄 Descripció de la Tasca

En aquesta tasca treballo com a consultor d’**EverPia** amb l’objectiu d’optimitzar la gestió d’impressores d’un client, **DevOptimize Solutions**. La problemàtica és clara: la gestió d’impressores en oficines sol ser caòtica, amb drivers incompatibles, costos elevats i manca de control.

La solució professional que proposo és la implementació d’un **Servidor d’Impressió Centralitzat amb Linux**, capaç de gestionar impressores i compartir-les de manera transparent amb clients Linux, reduint costos i simplificant l’administració.

---

## 🎯 Objectiu de la Prova de Concepte (PoC)

Abans d’invertir en impressores de xarxa físiques, el client vol validar que:

- Un **servidor Linux (Ubuntu Server)** pot actuar com a servidor d’impressió.
- Els **clients Zorin OS** poden enviar feines d’impressió de manera remota.
- La solució és estable, centralitzada i fàcil de gestionar.

Per fer-ho sense hardware real, utilitzo **cups-pdf**, una impressora virtual que genera fitxers **PDF** al servidor en lloc d’imprimir en paper.

---

## 🧱 Escenari de Treball

Faig servir el mateix escenari utilitzat a la PoC de NFS:

### 🖥️ Màquina 1 — Servidor
- **Sistema operatiu:** Ubuntu Server  
- **Xarxes:**
  - NAT
  - Host-Only  
- **Rol:** Servidor d’impressió amb CUPS

### 💻 Màquina 2 — Client
- **Sistema operatiu:** Zorin OS (Desktop)  
- **Xarxes:** Mateixa configuració que el servidor  
- **Rol:** Client que envia feines d’impressió

---

## 🔬 Prova de Concepte (PoC)

Durant aquesta tasca documento pas a pas tot el procés per demostrar el correcte funcionament del servidor d’impressió.

### 1️⃣ Instal·lació de CUPS al servidor
Instal·lo el servei **CUPS (Common UNIX Printing System)** a l’Ubuntu Server i comprovo que el servei queda actiu i funcionant correctament.

### 2️⃣ Instal·lació de la impressora virtual
Configuro la impressora virtual **cups-pdf**, que simula una impressora de xarxa i desa els documents impresos en format PDF al servidor.

### 3️⃣ Configuració de CUPS
- Activo l’administració web de CUPS.
- Permeto que CUPS escolti per **totes les interfícies de xarxa**, no només localhost.
- Ajusto els permisos necessaris per a la gestió remota.

### 4️⃣ Compartició de la impressora
Accedeixo al frontal web de CUPS mitjançant el navegador i:
- Comparteixo la impressora virtual a la xarxa.
- Verifico que és visible per als clients.

### 5️⃣ Afegir la impressora al client Zorin
Des del client Zorin OS:
- Detecto la impressora compartida pel servidor.
- L’afegeixo al sistema com si fos una impressora de xarxa real.

### 6️⃣ Proves d’impressió
Envio diversos documents des del client Zorin:
- Documents de text
- Fitxers PDF
- Altres documents de prova

### 7️⃣ Verificació al servidor
Comprovo al servidor que:
- S’han generat correctament els fitxers **PDF**.
- Cada fitxer correspon a una feina d’impressió enviada pel client.

---

## ✅ Resultat Final

Amb aquesta tasca he demostrat que:

- Un **Ubuntu Server amb CUPS** pot actuar com a servidor d’impressió centralitzat.
- Els **clients Zorin OS** poden imprimir de manera transparent.
- No és necessari invertir inicialment en impressores de xarxa físiques.
- La solució és escalable, professional i adequada per a entorns empresarials.

Aquesta PoC valida una proposta realista per centralitzar la impressió i millorar la gestió IT del client.

---

## 📚 Materials i Recursos

- Material propi: **UD5. AA1. CUPS** (Moodle de Sistemes Operatius en Xarxa)
- Vídeo: *Instalación de servidor de impresión en CUPS para Linux*  
  J.B. Alex Mantich (2024) — YouTube  
  https://www.youtube.com/watch?v=FNwSTrOSgZQ
- Documentació Ubuntu Server (Canonical):  
  https://documentation.ubuntu.com/server/
- Guia instal·lació CUPS Ubuntu 24.04:  
  https://idroot.us/install-cups-print-server-ubuntu-24-04/


---

[Guia de la tasca](Guia.md)

[Tornar a la pàgina principal](../)
