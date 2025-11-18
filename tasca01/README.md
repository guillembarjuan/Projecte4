# 📦 P07: DRP – Còpies de Seguretat. Estudi Cas Client (Treball Cooperatiu)

## 📄 Breu descripció
En aquesta tasca hem treballat de manera cooperativa per dissenyar una **política de còpies de seguretat** per a l’empresa **“Muntatges i Serveis Tècnics SL”**, una petita empresa dedicada a la instal·lació i manteniment d’equips industrials. L’objectiu principal és garantir la **seguretat, integritat i disponibilitat** de les dades crítiques del client davant qualsevol incidència.

Durant la primera hora, el nostre responsable de seguretat ens ha presentat el tema de les còpies de seguretat a partir d’un material didàctic. Posteriorment, hem aplicat aquests coneixements en una **dinàmica cooperativa** basada en un cas real.

## 🏢 Cas Client
L’empresa disposa de la següent infraestructura tècnica:

- **Servidor de fitxers (Ubuntu Server)**:
  - Documents de projectes: 300 GB, creixement moderat.
  - Bases de dades de comptabilitat i clients: 20 GB, ús diari i canvi constant.
  - Carpetes personals dels usuaris: 100 GB, ús diari.
- **10 equips clients (Windows 10/11)**: Alguns usuaris guarden fitxers de forma temporal.
- **Connexió a Internet**: Fibra òptica simètrica de 600 Mbps.

### Requisits de Recuperació
- **RTO (Temps de Recuperació)**: Comptabilitat/Clients ha d’estar disponible en menys de 4 hores.
- **RPO (Pèrdua de Dades Admesa)**: 24 hores màximes per a la majoria de dades, 4 hores per Comptabilitat/Clients.
- **Retenció**: Historial d’almenys un mes.

## ⚙️ Desenvolupament de la Tasca

### Fase 1: Treball Individual
Cada membre ha respost individualment les preguntes següents:
1. **Què copiar?** Determinar les dades més crítiques i si cal incloure els 10 equips clients.
2. **Periodicitat i tipus de còpia**: Proposar un calendari setmanal i el tipus de còpia (Completa, Diferencial, Incremental).
3. **Mitjans i ubicació**: Seleccionar mitjans de còpia (Discs durs externs, NAS, Cloud, cintes) i aplicar la regla 3-2-1 per a la còpia més recent.

### Fase 2: Treball per Parelles
- **Discussió i Consens**: Comparació de respostes individuals.
- **Elaboració d’una Proposta Unificada**: Disseny d’un esquema 3-2-1 de còpies basat en els requisits del cas.

### Fase 3: Treball en Grup
- **Debat i Selecció**: Presentació i discussió de cada proposta per determinar els pros i contres.
- **Disseny de la Política Final**: Redacció de la **Política de Còpies de Seguretat Definitiva** que es presentarà a l’empresa.

## 📑 Document Final
El document que lliurem conté:
1. **Dades objecte de còpia**: Separant servidor/clients i crítiques/no crítiques.
2. **Cronograma setmanal detallat**:

| Dia       | Dades       | Tipus de còpia | Mitjà       |
|----------|------------|----------------|------------|
| Dilluns  |            |                |            |
| Dimarts  |            |                |            |
| ...      |            |                |            |
| Diumenge |            |                |            |

3. **Elecció de mitjans i ubicació (Regla 3-2-1)**:
   - Mitjà 1 (Local): Ex. Disc dur USB o NAS.
   - Mitjà 2 (Extern): Ex. Cloud, LTO, amb proveïdor específic.
   - Ubicació fora de lloc: Lloc físic o lògic, responsable de gestió.

4. **Estratègia de recuperació (RTO/RPO)**: Com garantim el compliment dels requisits de comptabilitat i clients.

## 📚 Materials i Enllaços de Suport
- Moodle 0226 Seguretat Informàtica. RA2.AA3Còpies  
- INCIBE: [Copias de seguridad. Una guía de aproximación para el empresario](https://www.incibe.es/)  
- Xataka: [Backup 3-2-1, el método definitivo para mantener a salvo tus datos](https://youtu.be/PM_M4Iz6I4o?si=F7DRyDDTZE3hjWn8) (YouTube, Setembre 2017)

---

[Solució de la tasca](solucio.md)

---

[Tornar a la pàgina principal](../)

