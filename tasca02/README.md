# 💾 T02: DPR — Còpies de Seguretat. Cas Pràctic

## 📄 Descripció de la Tasca

En aquesta tasca portem a la pràctica la política de còpies de seguretat dissenyada prèviament per al client **“Muntatges i Serveis Tècnics SL”**. Després de definir l’estratègia teòrica (RTO, RPO, esquema 3-2-1 i mitjans), el client ens demana **guies tècniques amb proves de concepte** perquè el seu personal sigui capaç d’implantar i gestionar correctament el pla de còpies de seguretat.

El nostre objectiu és demostrar, de manera pràctica i documentada, com aplicar polítiques de backup tant en **equips Windows** com en **servidors Linux**, utilitzant eines professionals i habituals en entorns reals.

---

## 🧩 Objectiu General

Com a equip d’EverPia, hem de:

- Implementar còpies de seguretat reals seguint l’esquema **3-2-1**.
- Crear **guies tècniques clares i reutilitzables**.
- Validar les còpies mitjançant **proves de restauració**.
- Automatitzar processos per garantir fiabilitat i seguretat.

---

# 🖥️ Part 1: Còpia de Seguretat d’Equips Clients Windows

Tot i que el DPR no contempla habitualment còpies locals dels equips clients, en aquest cas fem una **excepció** amb l’equip Windows del **director de l’empresa**, que conté informació sensible que no es vol centralitzar al servidor de fitxers.

### Enfocament de la solució

- Definim una política de còpies segons l’esquema **3-2-1**:
  - **1 còpia local** en un disc secundari.
  - **1 còpia externa al núvol** (Google Drive).
- Utilitzem l’eina **Duplicati** com a solució de backup.

### Prova de concepte

Per documentar el procés:

- Treballem amb una **màquina virtual Windows 11**.
- Configurem:
  - Un disc principal amb el sistema operatiu.
  - Un disc secundari de **10 GB** per a les còpies locals.
- Simulem el núvol amb un **compte de Google Drive no corporatiu**.

### Objectius de la part Windows

- Definir una còpia del perfil d’usuari:
  - Cada **hora** al disc secundari.
  - Cada dia a les **18:00** al núvol.
- Documentar:
  - Instal·lació i configuració de Duplicati.
  - Funcionament dels plans de còpia.
  - Restauració de dades des del disc local.
  - Restauració de dades des del núvol.

---

# 🐧 Part 2: Còpia de Seguretat del Servidor Linux

Per al servidor Linux, la solució proposada és **Duplicity**, una eina potent per a còpies locals i remotes, combinada amb el programador de tasques **cron**.

### Entorn de treball

- Màquina virtual amb **Ubuntu Server**.
- Segon disc de **10 GB** que simula una unitat externa de backup.

### Prova de concepte

En aquesta part demostrem:

- Preparació i muntatge manual de la unitat de backup.
- Còpies de seguretat de la carpeta `/home`.
- Restauració de dades esborrades.
- Diferència entre còpies completes i incrementals.

---

## ⚙️ Automatització i Seguretat

Un aspecte clau de la tasca és la **seguretat del procés de backup**. La unitat de còpia:

- Ha d’estar **desmuntada per defecte**.
- Només es munta durant l’execució de la còpia.
- Es desmunta automàticament en acabar.

Per això:

- Creem **scripts de còpia completa i incremental**.
- Fem servir la variable d’entorn `PASSPHRASE` per protegir les còpies.
- Programem l’execució automàtica amb **cron**:
  - Còpia completa setmanal.
  - Còpies incrementals diàries.

---

# 📦 Resultat Final de la Tasca

En finalitzar aquesta tasca, com a grup hem lliurat:

- Guies tècniques completes de:
  - Còpies de seguretat en **Windows amb Duplicati**.
  - Còpies de seguretat en **Linux amb Duplicity**.
- Proves documentades de:
  - Execució de còpies.
  - Restauració de dades.
- Automatització funcional amb scripts i cron.
- Una solució realista, segura i aplicable a un entorn empresarial.

Aquesta tasca consolida la nostra capacitat per **dissenyar, implementar i validar plans de còpies de seguretat professionals**, un pilar fonamental en qualsevol infraestructura IT.

---

## 📚 Materials i Recursos

- Duplicati: https://duplicati.com/  
- Duplicity (man page): http://manpages.ubuntu.com/manpages/trusty/man1/duplicity.1.html  
- Programació de tasques amb cron:  
  https://geekytheory.com/programar-tareas-en-linux-usando-crontab  
- Creació d’arxius de prova:
  - Windows: https://waytoit.wordpress.com/2015/03/15/creando-archivos-con-fsutil/
  - Linux: https://waytoit.wordpress.com/2015/03/21/creando-archivos-de-prueba-en-linux/

---

[Guia de linux](GuiaServer.md)

[Guia de windows](GuiaWindows.md)

[Tornar a la pàgina principal](../)
