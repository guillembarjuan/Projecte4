# T01: DRP: Còpies de Seguretat 🛡️

## Estudi de Cas Client (Treball Cooperatiu)

---

## Fase 1: Treball Individual

### Guillem Barjuan

**1. Què copiar? (Priorització)**
Quines són les dades més crítiques del servidor? Cal fer còpia dels 10 equips clients? Justifica-ho.

> Les dades més crítiques del servidor són aquelles que poden posar en perill les dades de clients, projectes de l’empresa i dades internes.
> No cal que es faci la còpia dels 10 equips de clients, ja que hi ha dades menys importants, però sí que s’hauria de copiar informació crítica, ja que la seva pèrdua podria ser molt perjudicial per a l’empresa i els clients.

**2. Periodicitat i Tipus de Còpia**
Proposta de calendari bàsic per a la setmana:

* **Diari:** còpia incremental, ràpida i eficient.
* **Setmanal:** còpia diferencial, copiant tot el contingut nou des de l’última còpia completa.
* **Mensual:** còpia completa, tot i que lenta, assegura un punt de restauració complet.

**3. Mitjans i Ubicació**
Segons la **regla 3-2-1**:

* **Mitjà Local:** disc dur extern o NAS, còpia ràpida i accessible.
* **Mitjà Extern:** núvol (Cloud), assegura protecció addicional.
* **Còpia fora de lloc:** una còpia s’ha de conservar fora de les instal·lacions, per protecció física.

---

### Miquel Vico

**1. Què copiar? (Priorització)**

> Les dades més crítiques del servidor són les **Bases de Dades (Comptabilitat i Clients)**. Són d’ús diari i han d’estar disponibles en menys de 4 hores.

**2. Periodicitat i Tipus de Còpia**

* **Diari:** incremental (ràpida, però recuperació més lenta).
* **Setmanal:** diferencial (dades modificades durant la setmana).
* **Mensual:** completa (totes les dades).

**3. Mitjans i Ubicació**

* **Regla 3-2-1:** SSD (original), disc dur i cinta magnètica (còpies).
* SSD per accés ràpid i edició, disc dur i cinta per còpia segura fora de la màquina principal.

---

## Fase 2: Treball per Parelles

### Guillem Barjuan i Miquel Vico

| Element                 | Proposta de la Parella                                                                                                                                                                                                                                                                                                                | Justificació                                                                                                                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Dades Crítiques**     | Les dades més crítiques del servidor són les que poden comprometre informació sensible de l’empresa i clients. Destaquen Bases de Dades de comptabilitat i clients, així com dades de projectes i informació interna. No és necessari copiar completament els 10 equips dels clients, però sí assegurar tota la informació important. | Hem seleccionat aquestes dades perquè la seva pèrdua podria afectar greument l’empresa i els clients. Centrar la còpia en la informació crítica protegeix el que és essencial i optimitza recursos. |
| **Periodicitat (BD)**   | Diàriament: còpia incremental. Setmanal: còpia diferencial. Mensual: còpia completa.                                                                                                                                                                                                                                                  | Incremental diària: ràpida i poc consum de recursos. Diferencial setmanal: volum intermedi i recuperació senzilla. Completa mensual: punt de restauració global i fiable.                           |
| **Tipus de Còpia (BD)** | Incremental (diària), Diferencial (setmanal), Completa (mensual).                                                                                                                                                                                                                                                                     | Equilibri entre velocitat, consum de recursos i seguretat. Incremental: ràpida; Diferencial: recuperació àgil; Completa: restauració total.                                                         |
| **Mitjà 1 (Local)**     | Disc dur extern o NAS                                                                                                                                                                                                                                                                                                                 | Còpia ràpida i accessible, restauracions immediates segons regla 3-2-1.                                                                                                                             |
| **Mitjà 2 (Extern)**    | Cloud                                                                                                                                                                                                                                                                                                                                 | Protecció fora de les instal·lacions, contra robatoris, incendis o altres desastres locals.                                                                                                         |

---

### Martí Codony i Marc Jurado

| Element                 | Proposta de la Parella                                       | Justificació                                                                                              |
| ----------------------- | ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- |
| **Dades Crítiques**     | Bases de dades del servidor i configuracions.                | Les bases de dades són sensibles i informació privada dels clients es pot restaurar amb imatge estàndard. |
| **Periodicitat (BD)**   | Incremental cada 4 hores i completa setmanal.                | Necessiten còpies freqüents per no perdre informació crítica.                                             |
| **Tipus de Còpia (BD)** | Incremental cada 4h, Diferencial setmanal, Completa mensual. | Ràpida i segura, permet restauració total.                                                                |
| **Mitjà 1 (Local)**     | NAS intern                                                   | Xarxa interna, restauracions ràpides.                                                                     |
| **Mitjà 2 (Extern)**    | Cloud + disc dur extern                                      | Protecció contra desastres físics i compliment regla 3-2-1.                                               |

---

## Fase 3: Treball en Grup

### 1) Dades Objecte de Còpia

Quines dades es copien i amb quina freqüència (Separant Servidor / Clients i crítiques / no crítiques):

* **Servidor:** Bases de dades i configuracions crítiques
* **Clients:** Dades crítiques segons prioritat
* **No crítiques:** dades menys rellevants

---

### 2) Cronograma Setmanal Detallat 📅

| Dia       | Dades                                                          | Tipus de còpia | Mitjà                         |
| --------- | -------------------------------------------------------------- | -------------- | ----------------------------- |
| Dilluns   | Bases de dades i configuracions crítiques                      | Incremental    | NAS / Disc dur extern         |
| Dimarts   | Bases de dades i configuracions crítiques                      | Incremental    | NAS / Disc dur extern         |
| Dimecres  | Bases de dades i configuracions crítiques                      | Incremental    | NAS / Disc dur extern         |
| Dijous    | Bases de dades i configuracions crítiques                      | Incremental    | NAS / Disc dur extern         |
| Divendres | Bases de dades i configuracions crítiques                      | Incremental    | NAS / Disc dur extern         |
| Dissabte  | Còpia diferencial setmanal de BD i dades crítiques clients     | Diferencial    | NAS / Disc dur extern + Cloud |
| Diumenge  | Còpia completa mensual (si toca) de tota la informació crítica | Completa       | Cloud / Cinta LTO             |

---

### 3) Elecció de Mitjans i Ubicació (Regla 3-2-1)

| Element               | Mitjà                                                        | Ubicació / Responsable                        | Justificació                                                                               |
| --------------------- | ------------------------------------------------------------ | --------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Mitjà 1 (Local)       | NAS intern o Disc dur extern                                 | Instal·lacions de l’empresa, IT               | Restauracions ràpides i disponibles immediatament                                          |
| Mitjà 2 (Extern)      | Cloud (Azure / Google Cloud) + Disc dur extern fora del lloc | Ubicació remota o proveïdor de cloud; IT      | Còpia fora de lloc, protegeix davant robatoris, incendis o desastres, complint regla 3-2-1 |
| Ubicació Fora de Lloc | Cloud / Disc dur extern en lloc remot                        | Proveïdor cloud o emmagatzematge físic extern | Assegura redundància i seguretat extra                                                     |

---

### 4) Estratègia de Recuperació (RTO/RPO) ⏱️

| Tipus de Dades                           | RPO (Recovery Point Objective) | RTO (Recovery Time Objective) | Estratègia                                                                        |
| ---------------------------------------- | ------------------------------ | ----------------------------- | --------------------------------------------------------------------------------- |
| Bases de dades crítiques i comptabilitat | 4 hores                        | 4 hores                       | Còpies incrementals cada 4 hores; restauració ràpida des de NAS o disc dur extern |
| Configuracions i dades crítiques clients | 24 hores                       | 4 hores                       | Còpies setmanals i cloud per restauració completa en cas de desastre              |
| Dades no crítiques                       | 7 dies                         | 24-48 hores                   | Còpies setmanals i mensuals, prioritzant eficiència i protecció bàsica            |

---

[Solució de la tasca](solucio.md)

---

[Tornar a la pàgina principal](../)

