# 🖥️ T07: Serveis d’Assistència Remota (Tasca en Parelles)

## 📄 Descripció de la Tasca

En aquesta tasca ens endinsem en un àmbit essencial de la feina real d’un consultor IT: el **suport directe a l’usuari final**. A EverPia, una gran part de les hores facturables provenen d’atendre incidències quotidianes com:

- “No se m’obre el PDF.”
- “M’ha desaparegut una icona.”
- “La impressora no imprimeix.”

En aquests casos, **no podem demanar al client configuracions complexes**, ni VPNs, ni obertura de ports. Necessitem una eina d’assistència remota **ràpida, segura, fiable i sota demanda**.  

La direcció d’EverPia vol estandarditzar una eina oficial d’ús intern i, per això, la nostra missió en parelles ha estat:

### **1️⃣ Fase 1 — Anàlisi comparativa i selecció de l’eina oficial**  
### **2️⃣ Fase 2 — Creació de les guies d’ús (per tècnics i per clients)**  

---

# 🧩 FASE 1 — Anàlisi Comparativa i Selecció de la Solució

A la primera fase hem dut a terme una anàlisi del mercat per identificar l’eina d’assistència remota que adoptarà EverPia de manera oficial. Hem estudiat quatre solucions àmpliament utilitzades:

1. **TeamViewer**
2. **AnyDesk**
3. **Chrome Remote Desktop**
4. **RustDesk** (opció open-source)

Hem analitzat aquestes eines segons criteris clau per a un departament Helpdesk professional:  
✔ facilitat d’ús per a l’usuari  
✔ compatibilitat entre sistemes  
✔ estabilitat de connexió  
✔ funcionalitats disponibles  
✔ model de preu i llicències  

A continuació, incloem la taula comparativa elaborada durant l'anàlisi.

---

## 🧮 Taula Comparativa

| Criteri / Eina                             | **TeamViewer**                                                                                                                                                  | **AnyDesk**                                                                                                         | **Chrome Remote Desktop**                                                                                                              | **RustDesk**                                                                                                   |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Facilitat d'ús (client)**                | **Molt alta** — QuickSupport sense instal·lació. Extremadament intuïtiu.                                                                                       | Alta — versió portable i simple.                                                                                    | Molt alta — funciona via navegador, però menys completa.                                                                                | Alta — portable, però menys coneguda.                                                                          |
| **Requereix instal·lació?**                | **No** per al mòdul QuickSupport.                                                                                                                              | No (portable).                                                                                                      | Sí (+ extensió Chrome).                                                                                                                | No obligatòriament.                                                                                            |
| **Dificultat per l’usuari**                | **Molt baixa**. Només comunicar ID i contrasenya.                                                                                                              | Baixa.                                                                                                              | Molt baixa, però necessita compte Google.                                                                                              | Baixa-mitjana.                                                                                                 |
| **Compatibilitat SO**                      | **Windows, macOS, Linux, Android, iOS, ChromeOS**                                                                                                              | Windows, macOS, Linux, Android, iOS                                                                                 | Windows, macOS, Linux, ChromeOS, Android, iOS                                                                                           | Windows, macOS, Linux, Android, iOS                                                                            |
| **Transferència d’arxius**                 | **Sí (molt avançada)**                                                                                                                                        | Sí                                                                                                                  | No (molt limitada)                                                                                                                      | Sí                                                                                                             |
| **Connexió sense supervisió**              | **Sí (molt fiable)**                                                                                                                                            | Sí                                                                                                                  | No real                                                                                                                                | Sí                                                                                                             |
| **Qualitat de connexió**                   | **Excel·lent**, la més robusta                                                                                                                               | Molt bona                                                                                                           | Bona però limitada                                                                                                                     | Bona                                                                                                           |
| **Model de preu**                          | Comercial. Gratuït només per ús personal.                                                                                                                      | Comercial (més econòmic).                                                                                           | Gratuït, però limitat i no dissenyat per suport professional.                                                                          | Gratuït / open-source                                                                                          |
| **Limitacions**                            | Cost elevat en entorns professionals.                                                                                                                          | Robustesa inferior a TeamViewer.                                                                                    | Funcionalitats molt limitades.                                                                                                         | Cal servidor propi per a self-hosting.                                                                         |

---

# 🏆 Recomanació Oficial per a EverPia

## **✔ Eina Recomanada: TEAMVIEWER**

Després de l’anàlisi comparativa, hem conclòs que **TeamViewer és la millor opció per a EverPia**, ja que ofereix l’equilibri perfecte entre:

- **Facilitat extrema per al client**, que només ha de llegir un ID.
- **Potència i funcionalitats professionals** per als tècnics.
- **Compatibilitat total entre sistemes operatius.**
- **Qualitat de connexió líder al sector**, fins i tot en xarxes molt restrictives.
- **Prestigi i reconeixement** com a estàndard industrial en suport tècnic.

Aquesta combinació fa que TeamViewer sigui l'opció òptima per al nostre flux de treball i per garantir una experiència professional i eficient.

---

# 🛠️ FASE 2 — Creació de les Guies d’Ús

Un cop seleccionada l’eina oficial (TeamViewer), hem creat dues guies diferenciades que EverPia utilitzarà en el seu flux de suport:

---

## 📘 Guia 1: Manual per al Tècnic (Intern EverPia)

Aquesta guia està pensada per als futurs becaris i consultors que s’incorporaran a l’empresa.

Inclou:

- Instal·lació de la versió completa per a tècnics.  
- Com iniciar una sessió amb un client.  
- Gestió funcional de la sessió:  
  - transferència d’arxius  
  - canvi de pantalla  
  - reinici remot  
- Bones pràctiques de seguretat:
  - tancar sessió sempre  
  - no desar credencials  
  - validació d’identitat del client  

---

## 📗 Guia 2: Manual per al Client (Usuari Final)

Aquesta documentació està pensada per ser enviada al client en el moment d’una incidència.

La guia explica de manera extremadament simple i visual:

- Com descarregar el mòdul **TeamViewer QuickSupport**.  
- On cal fer clic en cada pas.  
- Com veure i comunicar l’ID i la contrasenya.  
- Com acceptar la connexió del tècnic.

L’objectiu és que fins i tot un usuari sense coneixements tècnics pugui completar el procés en menys d’un minut.

---

# 📚 Materials Utilitzats

- https://www.genbeta.com/a-fondo/que-software-instalar-a-tus-familiares-amigos-para-darles-soporte-ayuda-remoto  
- https://www.genbeta.com/herramientas/necesitas-escritorio-remoto-puedes-decirle-adios-a-teamviewer-rustdesk-gratis-e-ideal-para-usar-pc-movil  
- https://www.genbeta.com/herramientas/chrome-remote-desktop-que-como-funciona-como-puedes-usarlo-para-controlar-tu-pc-forma-remota  

---


