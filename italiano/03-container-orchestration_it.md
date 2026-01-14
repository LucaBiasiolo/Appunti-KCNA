# 03. Orchestrazione dei Container
## Uso dei Container
Mentre lo sviluppatore conosce meglio la sua applicazione e le sue dipendenze, di solito è un amministratore di sistema che fornisce l'infrastruttura, installa tutte le dipendenze e configura il sistema su cui gira l'applicazione. Questo processo può essere molto soggetto a errori e difficile da mantenere, quindi i server vengono configurati solo per un unico scopo come l'esecuzione di un database o di un server applicativo e poi vengono collegati tramite rete.
Le macchine virtuali possono essere utilizzate per emulare un intero server con cpu, memoria, storage, networking, sistema operativo e il software superiore. Ciò consente di eseguire più server isolati sullo stesso hardware. Ma poiché è necessario eseguire un intero sistema operativo incluso il kernel, ***comporta sempre un certo sovraccarico*** se è necessario eseguire molti server.
***I container possono essere utilizzati per risolvere entrambi questi problemi, gestendo le dipendenze di un'applicazione ed eseguendole in modo molto più efficiente rispetto all'avvio di molte macchine virtuali.***

## Nozioni di Base sui Container
Contrariamente alla credenza popolare, le tecnologie dei container sono molto più vecchie di quanto ci si aspetterebbe. Uno dei primi antenati delle moderne tecnologie dei container è il comando **chroot** introdotto in Version 7 Unix nel 1979. Il comando **chroot** poteva essere utilizzato per isolare un processo dal filesystem root e fondamentalmente "nascondere" i file al processo e simulare una nuova directory root. L'ambiente isolato è una cosiddetta *chroot jail*, dove i file non possono essere accessibili dal processo, ma sono ancora presenti sul sistema.
**Per isolare un processo ancora più di quanto possa fare chroot, gli attuali kernel Linux forniscono funzionalità come *namespaces* e cgroups.**
I **Namespaces** sono usati per isolare varie risorse, per esempio la rete. Un namespace di rete può essere usato per fornire una completa astrazione delle interfacce di rete e delle tabelle di routing.
Il Kernel Linux 5.6 attualmente fornisce 8 namespace:
- **pid** - l'ID del processo fornisce a un processo il proprio set di ID di processo.
- **net** - la rete consente ai processi di avere il proprio stack di rete, incluso l'indirizzo IP.
- **mnt** - mount astrae la vista del filesystem e gestisce i punti di montaggio.
- **ipc** - la comunicazione inter-processo fornisce la separazione dei segmenti di memoria condivisa nominati.
- **user** - fornisce ai processi il proprio set di ID utente e ID gruppo.
- **uts** - Unix time sharing consente ai processi di avere il proprio hostname e nome di dominio.
- **cgroup** - un namespace più recente che consente a un processo di avere il proprio set di directory root cgroup.
- **time** - il namespace più recente può essere usato per virtualizzare l'orologio del sistema.

I **cgroups** sono usati per organizzare i processi in gruppi gerarchici e assegnare loro risorse come memoria e CPU.
***Mentre le macchine virtuali emulano una macchina completa, incluso il sistema operativo e un kernel, i container condividono il kernel della macchina host e, come spiegato, sono solo processi isolati.***
Le macchine virtuali comportano un certo sovraccarico, sia esso il tempo di avvio, le dimensioni o l'utilizzo delle risorse per eseguire il sistema operativo. ***I container d'altra parte sono letteralmente processi***, come il browser che puoi avviare sulla tua macchina, quindi si avviano molto più velocemente e hanno un'impronta minore.

## Esecuzione dei Container
Per eseguire i container standard del settore, non è necessario utilizzare Docker; è possibile semplicemente seguire invece lo standard OCI [runtime-spec](https://github.com/opencontainers/runtime-spec).
L'Open Container Initiative mantiene anche un'implementazione di riferimento del runtime del container chiamata [runC](https://github.com/opencontainers/runc).
Per la tua macchina locale, ci sono molte alternative tra cui scegliere, alcune delle quali servono solo per costruire immagini come [buildah](https://buildah.io/) o [kaniko](https://github.com/GoogleContainerTools/kaniko), mentre altre si presentano come alternative complete a Docker, come fa [podman](https://podman.io/).

## Costruzione delle Immagini dei Container
Perché un container viene chiamato container in primo luogo? La metafora usata qui mira all'uso dei container per spedizioni che sono standardizzati secondo [ISO 668](https://en.wikipedia.org/wiki/ISO_668).
Docker ha riutilizzato tutti i componenti per isolare i processi come namespace e cgroups, ma un pezzo cruciale del puzzle che ha aiutato i container a raggiungere la loro svolta è l'introduzione delle *immagini dei container. *Docker descrive l'immagine di un container come segue:
"*Un'immagine di container Docker è un pacchetto software leggero, autonomo ed eseguibile che include tutto il necessario per eseguire un'applicazione: codice, runtime, strumenti di sistema, librerie di sistema e impostazioni."*
Le immagini consistono in un bundle del filesystem e metadati.
Le immagini possono essere costruite leggendo le istruzioni da un file di build chiamato *Dockerfile***.**

## Sicurezza
Quando i container vengono avviati su una macchina, condividono sempre lo stesso kernel, che diventa quindi un rischio per l'intero sistema.
***Uno dei maggiori rischi per la sicurezza è l'esecuzione di processi con troppi privilegi***, specialmente l'avvio di processi come root o amministratore.
Le ***4C della sicurezza Cloud Native*** possono dare un'idea approssimativa di quali livelli devono essere protetti se si utilizzano i container:
- **Codice** (Code)
- **Container**
- **Cluster**
- **Cloud **(/Co-Lo/Corporate Datacenter)


## Fondamenti dell'Orchestrazione dei Container
Avere molti piccoli container debolmente accoppiati, isolati e indipendenti è la base per le cosiddette architetture a microservizi.
Se devi gestire e distribuire grandi quantità di container, arrivi rapidamente al punto in cui hai bisogno di un sistema che aiuti nella gestione di questi container. I problemi da risolvere possono includere:
- **Fornire risorse di calcolo** come macchine virtuali su cui i container possono essere eseguiti
- **Pianificare** (Schedule) i container sui server in modo efficiente
- **Allocare risorse** come CPU e memoria ai container
- **Gestire la disponibilità** dei container e sostituirli se falliscono
- **Scalare** i container se il carico aumenta
- **Fornire networking** per connettere i container insieme
- **Predisporre storage** se i container devono persistere i dati

La maggior parte dei sistemi di orchestrazione dei container consiste in **due parti**: un ***control plane responsabile della gestione dei container*** e dei ***worker node che ospitano effettivamente i container***.

## Networking
I namespace di rete consentono a ogni container di avere il proprio indirizzo IP univoco, quindi più applicazioni possono aprire la stessa porta di rete.
Per rendere l'applicazione accessibile dall'esterno del sistema host, i container hanno la possibilità di mappare una porta dal container a una porta dal sistema host.
Per consentire la comunicazione tra container attraverso gli host, possiamo usare una rete overlay che li inserisce in una rete virtuale estesa attraverso i sistemi host.
La maggior parte delle implementazioni moderne del networking dei container si basa sulla Container Network Interface (CNI).
![image](..\files\image_g.png)

## Service Discovery & DNS
Nelle piattaforme di orchestrazione dei container le cose sono molto più complicate rispetto ai datacenter tradizionali:
- Centinaia o migliaia di container con indirizzi IP individuali
- I container sono distribuiti su una varietà di host diversi, datacenter diversi o persino geolocalizzazioni
- I container o i servizi hanno bisogno del DNS per comunicare. L'uso degli indirizzi IP è quasi impossibile
- Le informazioni sui container devono essere rimosse dal sistema quando vengono eliminati.

La soluzione al problema è ancora una volta l'automazione, tutte le informazioni vengono inserite in un ***Service Registry***. **Trovare altri servizi nella rete e richiedere informazioni su di essi è chiamato *Service Discovery***.
Approcci alla Service Discovery:
- **DNS** I moderni server DNS che dispongono di un'API di servizio possono essere utilizzati per registrare nuovi servizi man mano che vengono creati
- **Key-Value-Store** Utilizzo di un datastore fortemente coerente specialmente per memorizzare informazioni sui servizi (**etcd**, **Consul**, **Apache Zookeeper**)


## Service Mesh
Il monitoraggio, il controllo degli accessi o la crittografia del traffico di rete sono desiderabili quando i container comunicano tra loro.
Invece di implementare tutta questa funzionalità nella tua applicazione, puoi semplicemente avviare un secondo container che ha questa funzionalità implementata.
Il software che puoi usare per gestire il traffico di rete è chiamato **proxy**. Si tratta di un'applicazione server che si trova tra un client e un server e può modificare o filtrare il traffico di rete prima che raggiunga il server. Rappresentanti popolari sono [nginx](https://www.nginx.com/), [haproxy](http://www.haproxy.org/) o [envoy](https://www.envoyproxy.io/).
***Una service mesh aggiunge un server proxy a ogni container che hai nella tua architettura.***
![image](..files\image_s.png)
Quando viene utilizzata una service mesh, le applicazioni non parlano tra loro direttamente, ma il traffico viene instradato attraverso i proxy. Le service mesh più popolari al momento sono [istio](https://istio.io/) e [linkerd](https://linkerd.io/).
I proxy in una service mesh formano il ***data plane***. Qui è dove le regole di rete sono implementate e modellano il flusso del traffico.
Queste regole sono gestite centralmente nel ***control plane*** della service mesh. Qui è dove definisci come fluisce il traffico dal servizio A al servizio B e quale configurazione dovrebbe essere applicata ai proxy.
Quindi, invece di scrivere codice e installare librerie, scrivi semplicemente un file di configurazione in cui dici alla service mesh che il servizio A e il servizio B dovrebbero sempre comunicare in modo crittografato.
La configurazione viene quindi caricata sul control plane e distribuita al data plane per far rispettare la nuova regola.

## Storage
Dal punto di vista dello storage, i container hanno un difetto significativo: sono effimeri.
Le immagini dei container sono in sola lettura e consistono in diversi strati che includono tutto ciò che hai aggiunto durante la fase di build. Ciò garantisce che ogni volta che avvii un container da un'immagine ottieni lo stesso comportamento e funzionalità.
Per consentire la scrittura di file, **uno strato di lettura-scrittura viene posto sopra l'immagine del container** quando si avvia un container da un'immagine.
![image](..files\image_8.png)
Il problema qui è che **questo strato di lettura-scrittura viene perso quando il container viene fermato o eliminato**.
Se un container deve persistere i dati su un host, un ***volume*** può essere utilizzato per ottenere ciò: invece di isolare l'intero filesystem di un processo, le directory che risiedono sull'host vengono passate nel filesystem del container, ***questo indebolisce l'isolamento del container, dando accesso al filesystem dell'host***.
Spesso, i dati devono essere accessibili da più container che vengono avviati su diversi sistemi host o quando un container viene avviato su un host diverso dovrebbe comunque avere accesso al suo volume.
I sistemi di orchestrazione dei container come Kubernetes possono aiutare a mitigare questi problemi, ma richiedono sempre un sistema di storage robusto collegato ai server host.
![image](..files\image_o.png)
