# 07. Osservabilità Cloud Native (Cloud Native Observability)
## Osservabilità (Observability)
Il termine *osservabilità* è strettamente correlato alla [teoria del controllo](https://en.wikipedia.org/wiki/Control_theory) che si occupa del comportamento dei sistemi dinamici. In sostanza, ***la teoria del controllo descrive come gli output esterni dei sistemi possano essere misurati per manipolare il comportamento del sistema stesso***.
L'obiettivo superiore dell'osservabilità è consentire l'**analisi** dei dati raccolti.

## Telemetria (Telemetry)
Il termine *telemetria* ha radici greche e significa remoto o distanza (tele) e misurazione (metry).
Misurare e raccogliere punti dati per poi trasferirli a un altro sistema non è ovviamente esclusivo dei sistemi cloud native o persino IT.
Nei sistemi a container, ogni singola applicazione dovrebbe avere strumenti integrati che generano dati informativi, che vengono poi raccolti e trasferiti in un sistema centralizzato. I dati possono essere divisi in tre categorie:
- **Logs** messaggi emessi da un'applicazione quando devono essere presentati errori, avvisi o informazioni di debug
- **Metriche (Metrics)** misurazioni quantitative effettuate nel tempo
- **Tracce (Traces)** tracciano la progressione di una richiesta mentre passa attraverso il sistema


## Logging
I programmi Unix e Linux forniscono tre stream di I/O, di cui due possono essere utilizzati per emettere log da un container:
- standard input (stdin): Input a un programma, ad esempio tramite tastiera
- standard output (stdout): L'output che un programma scrive sullo schermo
- standard error (stderr): Errori che un programma scrive sullo schermo

Gli strumenti a riga di comando come **docker**, **kubectl** o **podman** forniscono un comando per mostrare i log dei processi containerizzati se li lasci loggare direttamente sulla console o su **/dev/stdout** e **/dev/stderr**:

*docker logs [nome container]*

Kubernetes fornisce la stessa funzionalità con lo strumento a riga di comando **kubectl**:

*kubectl logs [nome pod]*

Altri esempi di logging:
- Restituisce lo snapshot dei log del precedente container ruby terminato dal pod web-1

    *kubectl logs -p -c ruby web-1*

- Inizia lo streaming dei log del container ruby nel pod web-1

    *kubectl logs -f -c ruby web-1*

- Visualizza solo le 20 righe più recenti dell'output nel pod nginx

    *kubectl logs —tail=20 nginx*

- Mostra tutti i log dal pod nginx scritti nell'ultima ora

    *kubectl logs —since=1h nginx*

Per spedire (ship) i log, possono essere utilizzati diversi metodi:
- **Logging a livello di Node** (fluentd o filebeat)
Il modo più efficiente per raccogliere i log. Un amministratore configura uno strumento di spedizione log che raccoglie i log e li invia a un archivio centrale.
- **Logging tramite sidecar container** (fluentd o filebeat)
L'applicazione ha un sidecar container che raccoglie i log e li invia a un archivio centrale.
- **Logging a livello di applicazione**
L'applicazione invia i log direttamente all'archivio centrale. Sebbene all'inizio sembri molto conveniente, richiede la configurazione dell'adattatore di logging in ogni applicazione che gira in un Cluster.

Per rendere i log facili da elaborare e ricercabili, assicurati di loggare in un formato strutturato come JSON invece che in testo semplice.

## Prometheus
[Prometheus](https://prometheus.io/) è un sistema di monitoraggio open source, originariamente sviluppato presso SoundCloud, che è diventato il secondo progetto ospitato dalla CNCF nel 2016.
Prometheus può raccogliere metriche emesse da applicazioni e server come dati di serie temporali (time series data) e fornisce **4 metriche principali**:
- **Counter** un valore che aumenta, come il conteggio di richieste o errori
- **Gauge** valori che aumentano o diminuiscono, come la dimensione della memoria
- **Histogram** un campione di osservazioni, come la durata della richiesta o la dimensione della risposta
- **Summary** simile a un istogramma, ma fornisce anche il conteggio totale delle osservazioni

Per esporre queste metriche, le applicazioni possono esporre un endpoint HTTP sotto **/metrics** invece di implementarlo autonomamente.
Prometheus ha il supporto integrato per Kubernetes e può essere configurato per scoprire automaticamente tutti i Service nel tuo Cluster e raccogliere i dati delle metriche a un intervallo definito per ***salvarli in un database di serie temporali (time series database)***.
Per interrogare i dati memorizzati nel database di serie temporali, Prometheus fornisce un linguaggio di query chiamato [PromQL (Prometheus Query Language)](https://prometheus.io/docs/prometheus/latest/querying/basics/).
Il ***compagno più utilizzato per Prometheus è Grafana***, che può essere utilizzato per costruire dashboard dalle metriche raccolte.
***Un altro strumento dell'ecosistema Prometheus è l'[Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/)***. Il server Prometheus stesso ti consente di configurare avvisi quando determinate metriche raggiungono o superano una soglia.

## Tracing (Tracciamento)
Una traccia descrive il monitoraggio di una richiesta mentre passa attraverso i Service. Una traccia consiste in più unità di lavoro che rappresentano i diversi eventi che si verificano mentre la richiesta attraversa il sistema.
Queste tracce possono essere memorizzate e analizzate in un sistema di tracciamento come [Jaeger](https://www.jaegertracing.io/).
Il progetto OpenTelemetry è un insieme di interfacce di programmazione delle applicazioni (API), kit di sviluppo software (SDK) e strumenti che possono essere utilizzati per integrare la telemetria come metriche, protocolli, ma soprattutto tracce nelle applicazioni e nelle infrastrutture. I client OpenTelemetry possono essere utilizzati per esportare i dati di telemetria in un formato standardizzato verso piattaforme centrali come Jaeger.

## Gestione dei Costi (Cost Management)
Le possibilità del cloud computing ci permettono di attingere da un pool teoricamente infinito di risorse e pagarle solo quando sono realmente necessarie.
Esistono diversi modi per eseguire un'ottimizzazione automatica e manuale:
- **Identificare le risorse sprecate e inutilizzate** con un buon monitoraggio dell'utilizzo delle risorse, è molto facile trovare risorse inutilizzate o server che non hanno molto tempo di inattività. Molti fornitori di cloud hanno esploratori dei costi che possono suddividere i costi per i singoli servizi. L'Autoscaling aiuta a spegnere le istanze che non sono necessarie
- **Right-Sizing** può essere una buona idea scegliere server e sistemi con molta più potenza di quella effettivamente necessaria. Ancora una volta, un buon monitoraggio può darti indicazioni nel tempo su quante risorse sono effettivamente necessarie per la tua applicazione. Non comprare macchine potenti se ti serve solo metà della loro capacità
- **Istanze Riservate (Reserved Instances)** i modelli di prezzo on-demand sono ottimi se hai davvero bisogno di risorse su richiesta. Un metodo per risparmiare molti soldi è riservare le risorse e persino pagarle in anticipo
- **Istanze Spot (Spot Instances)** se hai un Job batch o un carico pesante per un breve periodo di tempo, puoi usare le istanze spot per risparmiare denaro. L'idea delle istanze spot è di ottenere risorse inutilizzate che sono state sovra-provisionate dal fornitore di cloud a prezzi molto bassi
