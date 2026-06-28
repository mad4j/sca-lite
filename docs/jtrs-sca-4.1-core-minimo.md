# JTRS SCA 4.1 — sottoinsieme minimo requisiti Core Framework

## Obiettivo
Definire il set minimo di requisiti necessari per un Core Framework **SCA 4.1 compliant**, eliminando tutto ciò che non è strettamente necessario alla compliance.

## Sottoinsieme minimale (MVP) per compliance

| ID interno | Requisito minimo | Perché è indispensabile | Tracciabilità SCA 4.1 (origine) |
|---|---|---|---|
| MIN-CF-01 | Presenza del **DomainManager** con lifecycle base (inizializzazione dominio e shutdown controllato). | È il punto di controllo del dominio SCA e coordina distribuzione/applicazioni. | SCA 4.1 Core Framework IDL: `CF::DomainManager` |
| MIN-CF-02 | Presenza del **DeviceManager** registrato nel dominio. | Senza gestione dispositivi non è possibile allocare/associare risorse hardware/software al dominio. | SCA 4.1 Core Framework IDL: `CF::DeviceManager` |
| MIN-CF-03 | Gestione applicazioni con operazioni minime: installazione/distribuzione, start, stop, rilascio/disinstallazione. | È il cuore operativo richiesto per eseguire waveform/applicazioni SCA. | SCA 4.1 Core Framework IDL: `CF::Application`, `CF::ApplicationFactory`, funzioni correlate del `DomainManager` |
| MIN-CF-04 | Supporto al modello componenti SCA (Resource/Device/Service) con interfacce IDL standard. | Garantisce portabilità e interoperabilità tra componenti conformi SCA. | SCA 4.1 Core Framework IDL: modulo `CF` (interfacce base componenti) |
| MIN-CF-05 | **Property management** standard: query/configure delle proprietà dei componenti. | Necessario per configurazione runtime e controllo componenti secondo API SCA. | SCA 4.1 Core Framework IDL: `CF::PropertySet` (e interfacce che lo estendono) |
| MIN-CF-06 | **Naming/lookup** degli oggetti Core Framework. | Necessario per scoprire e collegare gli oggetti distribuiti nel dominio. | SCA 4.1 (uso naming CORBA per risoluzione oggetti CF) |
| MIN-CF-07 | Accesso a file e pacchetti software per deploy (file manager minimo). | Il deployment richiede recupero descrittori/pacchetti software. | SCA 4.1 Core Framework IDL: interfacce file management (`CF::File*`) |

## Elenco puntuale dei singoli requisiti SCA minimi

Di seguito il dettaglio operativo dei requisiti minimi, espresso come singoli requisiti verificabili.

### A) Gestione dominio e dispositivi

1. **SCA-REQ-001 — DomainManager presente e raggiungibile**
   - Il dominio deve esporre un oggetto `CF::DomainManager` risolvibile tramite naming standard.
2. **SCA-REQ-002 — Lifecycle base del DomainManager**
   - Devono essere supportate inizializzazione del dominio e terminazione controllata.
3. **SCA-REQ-003 — DeviceManager registrato nel dominio**
   - Almeno un `CF::DeviceManager` deve potersi registrare al `DomainManager`.
4. **SCA-REQ-004 — Visibilità dei dispositivi gestiti**
   - Il dominio deve poter elencare/riferire i dispositivi gestiti dal `DeviceManager`.

### B) Gestione applicazioni (waveform)

5. **SCA-REQ-005 — Installazione/distribuzione applicazione**
   - Il dominio deve supportare la creazione/instanziazione applicazione tramite meccanismi `ApplicationFactory`.
6. **SCA-REQ-006 — Avvio applicazione**
   - Un’applicazione instanziata deve poter essere portata in esecuzione (`start`).
7. **SCA-REQ-007 — Arresto applicazione**
   - Un’applicazione in esecuzione deve poter essere fermata (`stop`).
8. **SCA-REQ-008 — Rilascio/disinstallazione applicazione**
   - Devono essere supportati cleanup e rilascio delle risorse applicative.

### C) Modello componenti e proprietà

9. **SCA-REQ-009 — Componenti conformi al modello CF**
   - Resource/Device/Service devono implementare le interfacce IDL CF applicabili.
10. **SCA-REQ-010 — Query delle proprietà**
    - I componenti devono esporre interrogazione proprietà via `CF::PropertySet::query`.
11. **SCA-REQ-011 — Configurazione delle proprietà**
    - I componenti devono supportare configurazione runtime via `CF::PropertySet::configure`.

### D) Naming e file management per deploy

12. **SCA-REQ-012 — Lookup oggetti CF**
    - Oggetti chiave (`DomainManager`, `DeviceManager`, `ApplicationFactory`, applicazioni) devono essere individuabili via naming.
13. **SCA-REQ-013 — Accesso file di deployment**
    - Deve essere disponibile accesso ai file necessari al deploy (descriptor/pacchetti) tramite interfacce CF file management.
14. **SCA-REQ-014 — Operazioni minime su file**
    - Devono essere disponibili almeno apertura/lettura/chiusura e navigazione minima dei contenuti richiesti dal deployment.

### Criterio di accettazione del sottoinsieme minimo

Il framework è considerato **minimamente conforme** quando tutti i requisiti `SCA-REQ-001 ... SCA-REQ-014` sono soddisfatti.

## Requisiti rimovibili (non necessari al sottoinsieme minimale)

I seguenti elementi possono essere rimossi in un framework minimale, purché non richiesti dal profilo operativo del prodotto:

1. Policy avanzate di pianificazione/affinità non richieste dalla specifica base.
2. Fault management avanzato (HA, ridondanza, failover automatico).
3. Ottimizzazioni performance non richieste dalla compliance.
4. Tooling di monitoraggio esteso oltre le API minime esposte da CF.
5. Adapter/protocolli non standard (estensioni vendor-specific).
6. Funzioni di orchestrazione multi-nodo avanzata se non parte del profilo target.

## Regola pratica di compliance minimale

Un requisito è mantenuto nel core **solo se**:

1. è necessario a istanziare e governare dominio/dispositivi/applicazioni SCA;
2. è richiesto da interfacce CF standard (IDL) usate per interoperabilità;
3. la sua rimozione impedirebbe l’esecuzione di un’applicazione SCA conforme.

In caso contrario, il requisito è candidato alla rimozione dal sottoinsieme minimale.
