# 02. Architettura Cloud Native
## Fondamenti dell'Architettura Cloud Native
La CNCF definisce il termine ***cloud native*** come segue:

"*Le tecnologie Cloud native consentono alle organizzazioni di creare ed eseguire applicazioni scalabili in ambienti moderni e dinamici come cloud pubblici, privati e ibridi. Container, service mesh, microservizi, infrastruttura immutabile e API dichiarative esemplificano questo approccio.*
*Queste tecniche abilitano sistemi debolmente accoppiati che sono resilienti, gestibili e osservabili. Combinati con una robusta automazione, consentono agli ingegneri di apportare cambiamenti ad alto impatto frequentemente e in modo prevedibile con il minimo sforzo.* *[…]*"

## Architettura Monolitica vs Microservizi

Architettura Monolitica: un'applicazione di solito ha una singola base di codice e viene fornita come un singolo file binario che può essere eseguito su un server
- tutte le funzionalità sono nella stessa applicazione
- facile da sviluppare e distribuire
- molto difficile da gestire la complessità, scalare tra più team, implementare cambiamenti velocemente o scalare in modo efficiente

Architettura a Microservizi: molteplici applicazioni disaccoppiate che comunicano tra loro in una rete
- le funzionalità sono fornite in molte applicazioni diverse
- facile da gestire la complessità
- facile mantenere le applicazioni
- operare e scalare le applicazioni individualmente
- scalare in modo efficiente e tra più team
- ogni team può avere la proprietà di diverse funzioni


## Caratteristiche dell'Architettura Cloud Native
Le caratteristiche dell'Architettura Cloud Native sono:
- **Alto livello di automazione** l'automazione in ogni fase, dallo sviluppo alla distribuzione, può essere ottenuta utilizzando strumenti di automazione moderni e pipeline CI/CD supportate da un Version Control System come git
- **Self healing** i framework delle applicazioni cloud native e i componenti dell'infrastruttura includono controlli di integrità (health checks) che aiutano a monitorare l'applicazione dall'interno e a riavviarla automaticamente se fallisce (o se necessario)
- **Scalabile** la scalabilità descrive il processo di gestione di un carico maggiore pur fornendo una piacevole esperienza utente (vedi Vertical/Horizontal scaling)
- **Costi efficienti** proprio come scalare verticalmente l'applicazione per situazioni di traffico elevato, scalare verso il basso l'applicazione e i modelli di prezzo basati sull'utilizzo dei fornitori di cloud possono far risparmiare costi se il traffico è basso
- **Facile da mantenere** l'utilizzo dei *Microservizi* consente di scomporre le applicazioni in pezzi più piccoli e renderle più portatili, più facili da testare e da distribuire tra più team
- **Sicuro per impostazione predefinita** gli ambienti cloud sono spesso condivisi tra più clienti o team, il che richiede diversi modelli di sicurezza; pattern come "zero trust computing" richiedono l'autenticazione da ogni utente e processo

> riferimento twelve-factor app (metodologia per la creazione di software-as-a-service) presso 12factor.net

> riferimento zero trust security model (approccio alla strategia, progettazione e implementazione dei sistemi IT) presso zero trust security model


## Autoscaling
Il pattern di autoscaling descrive la regolazione dinamica delle risorse in base alla domanda corrente. CPU e memoria sono le metriche ovvie su cui decidere quando scalare le applicazioni all'aumentare o al diminuire del carico, ma possono essere considerati anche altri metodi basati sul tempo o su metriche aziendali per scalare i propri servizi verso l'alto o verso il basso.
- **Vertical Scaling** descrive il cambiamento nelle dimensioni dell'hardware sottostante, che funziona solo entro certi limiti hardware per il bare metal, ma anche per le macchine virtuali
- **Horizontal Scaling** descrive il processo di generazione di nuove risorse di elaborazione che possono essere nuove copie del processo applicativo, macchine virtuali o - in modo meno immediato - persino nuovi rack di server e altro hardware

***Parlando di Autoscaling parliamo di Horizontal Scaling.***

## Serverless
**I fornitori di cloud suggeriscono che è molto facile distribuire applicazioni, ma richiedono di preparare e configurare diverse risorse come rete, macchine virtuali, sistemi operativi e load balancer per eseguire una semplice applicazione web. L'idea del serverless computing è quella di sollevare gli sviluppatori da questi compiti complicati.**
Tutti i popolari fornitori di cloud hanno una o più offerte commerciali di runtime serverless proprietari e un sottoinsieme di serverless, noto anche come Function as a Service (FaaS).
Il serverless computing ha un focus ancora maggiore sul provisioning e sulla scalabilità delle applicazioni on-demand. L'Autoscaling è un importante concetto fondamentale del serverless.
Sebbene ci siano molti vantaggi nella tecnologia serverless, inizialmente ha faticato con la standardizzazione. Per affrontare questi problemi, è stato fondato il progetto CloudEvents che fornisce una specifica su come i dati degli eventi dovrebbero essere strutturati. Gli eventi sono la base per la scalabilità dei carichi di lavoro serverless o per l'attivazione delle funzioni corrispondenti.
## **Standard Aperti**
Molte tecnologie cloud native si basano pesantemente sul software open source, un problema comune è come costruire e distribuire pacchetti software, i *container* si sono evoluti come un modo standardizzato per pacchettizzare e spedire software moderno.
**Open Container Initiative (OCI) **fornisce tre standard che definiscono il modo in cui costruire ed eseguire container:

- **image-spec** definisce come costruire e pacchettizzare immagini di container
- **runtime-spec** specifica la configurazione, l'ambiente di esecuzione e il ciclo di vita dei container
- **distribution-spec** (aggiunta più recente) fornisce uno standard per la distribuzione di contenuti in generale e immagini di container in particolare

Altri standard importanti sono:
- **Container Network Interface (CNI)** una specifica su come implementare il networking per i container
- **Container Runtime Interface (CRI)** una specifica su come implementare i runtime dei container nei sistemi di orchestrazione dei container
- **Container Storage Interface (CSI)** una specifica su come implementare lo storage nel sistema di orchestrazione dei container
- **Service Mesh Interface (SMI)** una specifica su come implementare le Service Mesh nei sistemi di orchestrazione dei container con focus su Kubernetes


## Ruoli Cloud Native & Site Reliability Engineering
I lavori nel cloud computing sono più difficili da descrivere e le transizioni sono più fluide, poiché le responsabilità sono spesso condivise tra più persone provenienti da aree diverse e con competenze diverse
Ruoli Cloud Native:
- **Cloud Architect** responsabile dell'adozione delle tecnologie cloud, della progettazione del panorama applicativo e dell'infrastruttura, con focus su sicurezza, scalabilità e meccanismi di distribuzione
- **DevOps Engineer** utilizza strumenti e processi che bilanciano lo sviluppo software e le operazioni
- **Security Engineer**
- **DevSecOps Engineer** combina i due ruoli precedenti, spesso usato per costruire ponti tra i team di sviluppo e sicurezza più tradizionali
- **Data Engineer** raccoglie, archivia e analizza le vaste quantità di dati che vengono o possono essere raccolti in grandi sistemi
- **Full-Stack Developer** tuttofare che si sente a casa sia nello sviluppo frontend che backend, così come nei dettagli dell'infrastruttura
- **Site Reliability Engineer** (SRE) crea e mantiene software affidabile e scalabile; per misurare le prestazioni e l'affidabilità, gli SRE utilizzano tre metriche principali:
    1. ***Service Level Objectives (SLO)*** "*Specifica un livello target per l'affidabilità del tuo servizio*" - es. Raggiungere una latenza inferiore a 100ms
    2. ***Service Level Indicators (SLI)*** "*Una misura quantitativa attentamente definita di alcuni aspetti del livello di servizio fornito" - *es. Quanto tempo impiega effettivamente una richiesta per ricevere risposta
    3. ***Service Level Agreements (SLA)*** "*Un contratto esplicito o implicito con gli utenti che include le conseguenze del rispetto (o del mancato rispetto) degli SLO che contengono. Le conseguenze sono più facilmente riconoscibili quando sono finanziarie – un rimborso o una penale – ma possono assumere altre forme*"


## Comunità e Governance
La CNCF ha un Technical Oversight Committee (TOC) responsabile della definizione e del mantenimento della visione tecnica, dell'approvazione di nuovi progetti, dell'accettazione dei feedback dal comitato degli utenti finali e della definizione di pratiche comuni che dovrebbero essere implementate nei progetti CNCF.
Allo stesso tempo, il TOC non controlla i progetti, ma li incoraggia a essere auto-gestiti e di proprietà della comunità e pratica il principio della “governance minima praticabile”.
Poiché i progetti open source dipendono dalle rispettive comunità, la governance è molto diversa dagli approcci tradizionali. La grande libertà delle tecnologie cloud native costringe le persone a imporre regole a se stesse e a farle rispettare.

## Criteri di Graduation CNCF v1.3
Ad ogni iniziativa CNCF è assegnato un livello di maturità. I progetti proposti alla CNCF dovrebbero specificare il loro grado di maturità preferito.
Un progetto deve ricevere una supermaggioranza di due terzi per essere approvato come incubating o graduated.
Eventuali voti graduated sono considerati come voti per unirsi come progetto incubating se non c'è una supermaggioranza di voti per farlo.
Eventuali voti graduated o incubating sono considerati come sponsorizzazione per entrare come progetto sandbox se non c'è una supermaggioranza di voti per entrare come progetto incubating.
Il progetto viene respinto se non ci sono abbastanza finanziamenti per qualificarlo per la fase sandbox.
***Fallback voting*** è il termine per questo metodo di scelta.
Livelli di Maturità del Progetto:
- **Sandbox Stage** questa fase è il punto di ingresso per i progetti in fase iniziale
- **Incubating Stage** il progetto per essere accettato alla fase di incubazione deve aver soddisfatto i requisiti della fase sandbox più altre verifiche tecniche che sono state eseguite come: numero sano di committers, usato in produzione da almeno 3 utenti, schema di versioning chiaro, processi di sicurezza documentati, ecc.
- **Graduation Stage** per graduare un progetto deve soddisfare i criteri della fase di incubazione più altri requisiti come: committers di almeno 2 organizzazioni, aver completato un audit di sicurezza indipendente di terze parti, avere un elenco pubblico di adottanti del progetto, ricevere il voto della supermaggioranza dal TOC per passare alla fase di graduation, ecc.
