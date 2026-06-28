# JTRS SCA 4.1 — sottoinsieme minimale requisiti Core Framework

## Obiettivo
Definire il set minimo di requisiti necessari per un Core Framework **SCA 4.1 compliant**, eliminando tutto ciò che non è strettamente necessario alla compliance.

## Sottoinsieme minimale (MVP) per compliance

| ID interno | Requisito minimo | Perché è indispensabile | Tracciabilità SCA 4.1 (origine) |
|---|---|---|---|
| MIN-CF-01 | Presenza del **DomainManager** con lifecycle base (inizializzazione dominio e shutdown controllato). | È il punto di controllo del dominio SCA e coordina deployment/applicazioni. | SCA 4.1 Core Framework IDL: `CF::DomainManager` |
| MIN-CF-02 | Presenza del **DeviceManager** registrato nel dominio. | Senza gestione dispositivi non è possibile allocare/associare risorse hardware/software al dominio. | SCA 4.1 Core Framework IDL: `CF::DeviceManager` |
| MIN-CF-03 | Gestione applicazioni con operazioni minime: install/deploy, start, stop, release/uninstall. | È il cuore operativo richiesto per eseguire waveform/applicazioni SCA. | SCA 4.1 Core Framework IDL: `CF::Application`, `CF::ApplicationFactory`, funzioni correlate del `DomainManager` |
| MIN-CF-04 | Supporto al modello componenti SCA (Resource/Device/Service) con interfacce IDL standard. | Garantisce portabilità e interoperabilità tra componenti conformi SCA. | SCA 4.1 Core Framework IDL: modulo `CF` (interfacce base componenti) |
| MIN-CF-05 | **Property management** standard: query/configure delle proprietà dei componenti. | Necessario per configurazione runtime e controllo componenti secondo API SCA. | SCA 4.1 Core Framework IDL: `CF::PropertySet` (e interfacce che lo estendono) |
| MIN-CF-06 | **Naming/lookup** degli oggetti Core Framework. | Necessario per scoprire e collegare gli oggetti distribuiti nel dominio. | SCA 4.1 (uso naming CORBA per risoluzione oggetti CF) |
| MIN-CF-07 | Accesso a file e pacchetti software per deploy (file manager minimo). | Il deployment richiede recupero descrittori/pacchetti software. | SCA 4.1 Core Framework IDL: interfacce file management (`CF::File*`) |

## Requisiti rimovibili (non necessari al core minimale)

I seguenti elementi possono essere rimossi in un framework minimale, purché non richiesti dal profilo operativo del prodotto:

1. Policy avanzate di scheduling/affinità non richieste dalla specifica base.
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

In caso contrario, il requisito è candidato alla rimozione dal core minimale.
