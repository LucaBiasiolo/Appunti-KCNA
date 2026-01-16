# 05. Lavorare con Kubernetes
## Oggetti Kubernetes (Kubernetes Objects)
Kubernetes fornisce molte risorse per lo più astratte, chiamate anche **oggetti** (objects).
Gli oggetti Kubernetes si possono distinguere tra **oggetti orientati al carico di lavoro** (workload-oriented objects) che vengono utilizzati per gestire i carichi di lavoro dei container e **oggetti orientati all'infrastruttura** (infrastructure-oriented objects) per gestire configurazione, networking e sicurezza. Alcuni di questi oggetti possono essere inseriti in un namespace, mentre altri sono disponibili in tutto il Cluster.
Gli oggetti sono solitamente descritti nel linguaggio YAML e inviati all'api-server, dove vengono validati prima di essere creati.
I campi richiesti sono:
- **apiVersion**
Ogni oggetto può essere versionato. Ciò significa che la struttura dei dati dell'oggetto può cambiare tra diverse versioni.
- **kind**
Il tipo di oggetto che deve essere creato.
- **metadata**
Dati che possono essere utilizzati per identificarlo. Un **name** è richiesto per ogni oggetto e deve essere unico. È possibile utilizzare i namespace se si ha bisogno di più oggetti con lo stesso nome.
- **spec**
La specifica dell'oggetto. Qui puoi descrivere il tuo stato desiderato. **La struttura dell'oggetto può cambiare con la sua versione!**


## Interagire con Kubernetes
Per accedere alle API, gli utenti possono utilizzare il client ufficiale dell'interfaccia a riga di comando chiamato **kubectl**.
Nonostante i numerosi strumenti CLI e GUI, esistono anche strumenti avanzati che consentono la creazione di template e il packaging degli oggetti Kubernetes. Probabilmente lo strumento più frequentemente utilizzato in connessione con Kubernetes oggi è [Helm](https://helm.sh/).
**Helm è un package manager per Kubernetes, che consente aggiornamenti e interazioni più semplici con gli oggetti. Helm pacchettizza gli oggetti Kubernetes in cosiddetti Charts, che possono essere condivisi con altri tramite un registro.**

## Concetto di Pod
**L'oggetto più importante in Kubernetes è un Pod. Un Pod descrive un'unità di uno o più container che condividono uno strato di isolamento di namespaces e cgroups.** È la più piccola unità distribuibile in Kubernetes, il che significa anche che Kubernetes non interagisce direttamente con i container. **Tutti i container all'interno di un Pod condividono un indirizzo IP e possono condividere tramite il filesystem**.
Potresti aggiungere tutti i container che vuoi alla tua applicazione principale. Ma fai attenzione perché perdi la capacità di scalarli individualmente! L'utilizzo di un secondo container che supporta la tua applicazione principale è chiamato **sidecar container**.
Tutti i container definiti vengono avviati contemporaneamente senza un ordine preciso, ma hai anche la possibilità di utilizzare **initContainers** per avviare container prima dell'avvio della tua applicazione principale.

## Ciclo di Vita del Pod (Pod Lifecycle)
I Pod seguono un ciclo di vita definito:
- **Pending** il Pod è stato accettato dal Cluster Kubernetes, ma uno o più container non sono stati configurati e resi pronti per l'esecuzione. Ciò include il tempo che un Pod trascorre in attesa di essere schedulato, così come il tempo trascorso a scaricare le immagini dei container sulla rete
- **Running** il Pod è stato vincolato a un Node e tutti i container sono stati creati. Almeno un container è ancora in esecuzione, o è in fase di avvio o riavvio
- **Succeeded** tutti i container nel Pod sono terminati con successo e non verranno riavviati
- **Failed** tutti i container nel Pod sono terminati e almeno un container è terminato con un fallimento. Ovvero, il container è uscito con uno stato diverso da zero o è stato terminato dal sistema
- **Unknown** per qualche motivo, non è stato possibile ottenere lo stato del Pod. Questa fase si verifica tipicamente a causa di un errore nella comunicazione con il Node in cui il Pod dovrebbe essere in esecuzione


## Oggetti Workload
Se un Pod viene perso perché un Node è fallito, è perso per sempre. Per assicurarsi che un numero definito di copie del Pod sia sempre in esecuzione, possiamo usare oggetti controller che gestiscono il Pod per noi.
Oggetti Kubernetes:
- **ReplicaSet** un oggetto controller che assicura che un numero desiderato di Pod sia in esecuzione in qualsiasi momento. I ReplicaSet ***possono essere utilizzati per scalare orizzontalmente le applicazioni*** e migliorarne la disponibilità.
- **Deployments** descrivono il ciclo di vita completo dell'applicazione, lo fanno ***gestendo molteplici ReplicaSet che vengono aggiornati quando l'applicazione viene modificata*** fornendo una nuova immagine del container, ad esempio. I Deployments sono ***perfetti per eseguire applicazioni stateless*** in Kubernetes
- **StatefulSet** può essere utilizzato per ***eseguire applicazioni stateful come i database***. Le applicazioni stateful hanno requisiti speciali che non si adattano alla natura effimera dei Pod e dei container. Cercano di mantenere gli indirizzi IP dei Pod e danno loro un nome stabile, storage persistente e una gestione più armoniosa della scalabilità e degli aggiornamenti
- **DaemonSet** assicura che una copia di un Pod sia in esecuzione su tutti (o alcuni) Node del tuo Cluster. ***Perfetto per eseguire carichi di lavoro relativi all'infrastruttura***, ad esempio strumenti di monitoraggio o logging
- **Job** crea uno o più Pod che eseguono un compito e terminano successivamente. Gli oggetti Job sono ***perfetti per eseguire script "one-shot"*** come migrazioni di database o attività amministrative
- **CronJob** aggiunge una configurazione basata sul tempo ai Job. Questo ***consente di eseguire i Job periodicamente***, ad esempio eseguendo un backup ogni notte alle 4 del mattino


## Oggetti di Networking
Poiché molti Pod richiederebbero molta configurazione manuale della rete, possiamo usare gli *oggetti Service e Ingress* per definire e astrarre il networking.
Tipi di Service:
- **ClusterIP** è un IP virtuale interno a Kubernetes che ***può essere utilizzato come singolo endpoint per un set di Pod***


![image](..files\image_x.png)
- **NodePort** estende il ClusterIP aggiungendo semplici regole di routing. Apre una porta (di default tra 30000-32767) su ogni Node nel Cluster e la mappa sul ClusterIP. Questo tipo di Service ***consente di instradare il traffico esterno verso il Cluster***
- **LoadBalancer** estende il NodePort distribuendo un'istanza di LoadBalancer esterno
- **ExternalName** tipo di Service speciale che non ha alcun routing. ExternalName utilizza il server DNS interno di Kubernetes per creare un alias DNS. Puoi usarlo per creare un semplice alias per risolvere un hostname piuttosto complicato. ***Questo è particolarmente utile se vuoi raggiungere risorse esterne dal tuo Cluster Kubernetes***
- **Headless Services** a volte non hai bisogno di load-balancing e di un singolo IP di Service. In questo caso, puoi creare quelli che vengono definiti Service "headless", specificando esplicitamente "None" per il Cluster IP. ***Puoi usare un Service headless per interfacciarti con altri meccanismi di service discovery, senza essere legato all'implementazione di Kubernetes***

Se hai bisogno di ancora più flessibilità per esporre le applicazioni, puoi usare un *oggetto Ingress*. ***Ingress fornisce un mezzo per esporre rotte HTTP e HTTPS dall'esterno del Cluster per un Service all'interno del Cluster.*** Lo fa configurando regole di routing che un utente può impostare e implementare con un *ingress controller*.
![image](..files\image_b.png)
Kubernetes fornisce anche un firewall interno al Cluster con il concetto di *NetworkPolicy*. Le NetworkPolicy sono un semplice IP firewall (OSI Layer 3 o 4) che può controllare il traffico in base a regole. Puoi definire regole per il traffico in entrata (ingress) e in uscita (egress). Un caso d'uso tipico per le NetworkPolicy sarebbe limitare il traffico tra due diversi Namespace.

## Oggetti Volume & Storage
I Volume consentono di condividere dati tra più container all'interno dello stesso Pod. Il secondo scopo che servono è prevenire la perdita di dati quando un Pod crasha e viene riavviato sullo stesso Node. I Pod vengono avviati in uno stato pulito, ma tutti i dati vanno persi a meno che non vengano scritti su un Volume.
Kubernetes utilizza la [Container Storage Interface (CSI)](https://github.com/container-storage-interface/spec) che consente a un fornitore di storage di scrivere un plugin (storage driver) che può essere utilizzato in Kubernetes.
Per utilizzare questa astrazione, abbiamo altri due oggetti che possono essere utilizzati:
- **PersistentVolumes (PV)**
***Una descrizione astratta per una fetta di storage***. La configurazione dell'oggetto contiene informazioni come tipo di Volume, dimensione del Volume, modalità di accesso e identificatori unici e informazioni su come montarlo.
- **PersistentVolumeClaims (PVC)**
***Una richiesta di storage da parte di un utente***. Se il Cluster ha più PersistentVolumes, l'utente può creare una PVC che riserverà un PersistentVolume in base alle esigenze dell'utente.

Dopo che il PersistentVolume è stato predisposto, uno sviluppatore può riservarlo con una PersistentVolumeClaim. L'ultimo passaggio consiste nell'utilizzare la PVC come Volume in un Pod.

## Oggetti di Configurazione (Configuration Objects)
È considerata una cattiva pratica incorporare la configurazione direttamente nella build del container. Qualsiasi modifica alla configurazione richiederebbe la ricostruzione dell'intera immagine e la ridistribuzione dell'intero container o Pod.
In Kubernetes, questo problema viene risolto disaccoppiando la configurazione dai Pod con una ConfigMap. Le ConfigMap possono essere utilizzate per memorizzare interi file di configurazione o variabili come coppie chiave-valore. Esistono due modi possibili per utilizzare una ConfigMap:
- **Montare una ConfigMap come Volume in un Pod**
- **Mappare le variabili da una ConfigMap alle variabili d'ambiente di un Pod**

Kubernetes fornisce anche i ***Secrets per memorizzare informazioni sensibili come password, chiavi o altre credenziali***. I Secrets sono strettamente correlati alle ConfigMap e fondamentalmente la loro unica differenza è che i Secrets sono codificati in base64.

## Oggetti di Autoscaling
Per scalare il carico di lavoro in un Cluster Kubernetes, possiamo usare tre diversi meccanismi di Autoscaling:
- **Horizontal Pod Autoscaler (HPA)** è l'autoscaler più utilizzato in Kubernetes. L'HPA ***può monitorare Deployment o ReplicaSet e aumentare il numero di repliche se viene raggiunta una determinata soglia***
- **Cluster Autoscaler** ovviamente, non ha senso avviare sempre più repliche di Pod se la capacità del Cluster è fissa. Il Cluster Autoscaler ***può aggiungere nuovi worker node al Cluster se la domanda aumenta***. Il Cluster Autoscaler funziona alla grande in tandem con l'Horizontal Autoscaler
- **Vertical Pod Autoscaler** ***consente ai Pod di aumentare dinamicamente le richieste e i limiti di risorse***. La scalabilità verticale è limitata dalla capacità del Node

L'autoscaling orizzontale in Kubernetes NON è disponibile "out of the box" e richiede l'installazione di un add-on chiamato [metrics-server](https://github.com/kubernetes-sigs/metrics-server).
[KEDA](https://keda.sh/) (**Kubernetes-based Event Driven Autoscaler**) può essere utilizzato per scalare il carico di lavoro di Kubernetes in base a eventi attivati da sistemi esterni. Simile all'HPA, KEDA può scalare Deployment, ReplicaSet, Pod, ecc., ma anche altri oggetti come i Job di Kubernetes. Con un'ampia selezione di scalers pronti all'uso, KEDA può scalare su trigger speciali come una query di database o persino il numero di Pod in un Cluster Kubernetes.

## Oggetti di Scheduling (Scheduling Objects)
Lo scheduler è il processo di controllo che assegna i Pod ai Node. Lo scheduler determina quali Node sono posizionamenti validi per ogni Pod nella coda di scheduling in base ai vincoli e alle risorse disponibili. Lo scheduler quindi classifica ogni Node valido e vincola il Pod a un Node adatto. ***kube-scheduler è l'implementazione predefinita***.
Esistono scenari in cui si desidera limitare il Pod a particolari Node o preferire l'esecuzione su particolari Node; i metodi principali sono:
- **Campo nodeSelector corrispondente alle label del Node** puoi aggiungere il campo nodeSelector alla specifica del tuo Pod e specificare le label del Node che desideri abbia il Node di destinazione. ***Kubernetes pianifica il Pod solo su Node che hanno ciascuna delle label specificate***
- **Affinità e anti-affinità (Affinity e anti-affinity)** puoi indicare che una regola è "soft" o preferita, in modo che lo ***scheduler pianifichi comunque il Pod anche se non riesce a trovare un Node corrispondente***
- **Campo nodeName** nodeName è un campo nella specifica del Pod; se non è vuoto, ***lo scheduler ignora il Pod e il kubelet (sul Node nominato) tenta di posizionare il Pod su quel Node, ignorando le regole nodeSelector o di affinità e anti-affinità***
- **Vincoli di diffusione della topologia del Pod (Pod topology spread constraints)** puoi usare i vincoli di diffusione della topologia per ***controllare come i Pod sono diffusi nel tuo Cluster tra i domini di fallimento come regioni, zone, Node***, o tra qualsiasi altro dominio di topologia definito

### Taint e toleration
La Node affinity è una proprietà dei Pod che li attrae verso un set di Node (come preferenza o come requisito rigido). ***I Taint sono l'opposto: consentono a un Node di respingere un set di Pod***.
Le tolerations sono applicate ai Pod. Le tolerations consentono allo scheduler di pianificare Pod con Taint corrispondenti. Le tolerations consentono la pianificazione, ma non la garantiscono: lo scheduler valuta anche altri parametri come parte della sua funzione.
***I Taint e le tolerations lavorano insieme per garantire che i Pod non vengano pianificati su Node inappropriati.***
Un Taint consiste in una chiave, un valore e un effetto. Come argomento qui, è espresso come **key=value:effect**.

## Sicurezza di Kubernetes
Poiché Kubernetes è un ambiente ad alta intensità di rete, è cruciale proteggere la rete utilizzando le tipiche tecniche di firewall dall'esterno del Cluster Kubernetes e utilizzando la crittografia pod-to-pod, una NetworkPolicy e altre misure dall'interno del Cluster Kubernetes.
### Accedere alle API
Per eseguire qualsiasi azione in un Cluster Kubernetes, è necessario accedere alle API e seguire tre passaggi principali.
### 1. Autenticazione (Authentication - token)
Il tipo di autenticazione utilizzato è definito nelle opzioni di avvio di kube-apiserver, di seguito sono riportati alcuni esempi di meccanismi di autenticazione:
```
--basic-auth-file

--oidc-issuer-url

--token-auth-file

--authorization-webhook-config-file
```
### 2. Autorizzazione (Authorization - RBAC)
**RBAC (Role-Based Access Control)** è il meccanismo di autorizzazione predefinito in Kubernetes che controlla chi può eseguire quali operazioni su quali risorse. RBAC si basa su un sistema di ruoli e permessi che permette di definire regole di accesso granulari e sicure.

#### Concetti fondamentali di RBAC

**Verbi (Operazioni)**
Le operazioni che possono essere eseguite sulle risorse Kubernetes sono definite come verbi:
- **create**: creare nuove risorse
- **read** (o **get**): leggere una singola risorsa
- **list**: elencare più risorse
- **watch**: osservare cambiamenti in tempo reale
- **update**: aggiornare una risorsa esistente
- **patch**: applicare modifiche parziali a una risorsa
- **delete**: eliminare una risorsa
- **deletecollection**: eliminare un'intera collezione di risorse

**Gruppi API**
Le risorse Kubernetes sono organizzate in gruppi API. Ogni gruppo contiene un insieme di risorse correlate:
- **core** (o **""**): risorse fondamentali come Pod, Service, ConfigMap, Secret, Namespace, Node
- **apps**: risorse per applicazioni come Deployment, StatefulSet, DaemonSet, ReplicaSet
- **batch**: risorse per job e cronjob
- **networking.k8s.io**: risorse di networking come Ingress, NetworkPolicy
- **storage.k8s.io**: risorse di storage come StorageClass, VolumeAttachment
- E molti altri...

#### Oggetti Kubernetes per RBAC

**1. Role**
Un **Role** definisce un insieme di permessi (regole) all'interno di un **singolo namespace**. Ogni regola specifica:
- Il gruppo API su cui agisce
- Le risorse all'interno di quel gruppo
- I verbi (operazioni) consentiti

**2. ClusterRole**
Un **ClusterRole** è simile a un Role, ma i suoi permessi si applicano a **tutto il cluster**. È utile per:
- Risorse a livello di cluster (Node, Namespace, PersistentVolume)
- Permessi che devono essere applicati in tutti i namespace
- Risorse che non appartengono a nessun namespace

**3. RoleBinding**
Un **RoleBinding** associa un Role a uno o più **soggetti** (utenti, gruppi o ServiceAccount) all'interno di un namespace specifico. È il meccanismo che "lega" i permessi definiti in un Role agli identificativi che li utilizzano.

**4. ClusterRoleBinding**
Un **ClusterRoleBinding** associa un ClusterRole a soggetti a **livello di cluster**. Questo permette di assegnare permessi cluster-wide a utenti, gruppi o ServiceAccount.

**5. ServiceAccount**
Un **ServiceAccount** rappresenta un'identità per processi che eseguono all'interno di un Pod. È l'unico tipo di "utente" che può essere creato direttamente come oggetto Kubernetes. I ServiceAccount sono utilizzati per:
- Autenticare processi interni al cluster (es. applicazioni che interagiscono con l'API server)
- Assegnare permessi specifici a Pod tramite RBAC
- Isolare le identità per diverse applicazioni

#### Soggetti in RBAC

I permessi possono essere assegnati a tre tipi di soggetti:
- **User Accounts**: identità utente umane. Non sono oggetti Kubernetes nativi, ma vengono gestiti esternamente (es. tramite certificati, token, provider di identità)
- **Service Accounts**: identità per processi interni al cluster, create come oggetti Kubernetes
- **Groups**: collezioni di utenti o ServiceAccount, utili per assegnare permessi a intere team

#### Flusso di autorizzazione

Quando un client (es. kubectl) tenta di eseguire un'operazione:
1. **Autenticazione**: verifica l'identità del client
2. **Autorizzazione RBAC**: il sistema controlla se esiste un RoleBinding/ClusterRoleBinding che assegna i permessi necessari al soggetto
3. **Admission Controllers**: controlli aggiuntivi possono modificare o negare la richiesta

#### Best practices

- **Principio del minimo privilegio**: assegnare solo i permessi strettamente necessari
- **Usare Role per namespace**: preferire Role a ClusterRole quando possibile
- **Separare i ruoli**: creare Role specifici per funzioni diverse (es. "reader", "developer", "admin")
- **Documentare i permessi**: mantenere una documentazione chiara dei ruoli e dei loro scopi
- **Audit regolare**: controllare periodicamente i permessi assegnati

RBAC è fondamentale per la sicurezza in Kubernetes, permettendo di controllare in modo granulare chi può fare cosa nel cluster.
### 3. Admission Controllers
Gli Admission controller sono pezzi di software che possono accedere al contenuto degli oggetti creati dalle richieste. Possono modificare il contenuto o convalidarlo, e potenzialmente negare la richiesta. Kubernetes fornisce diversi admission controller predefiniti che possono essere abilitati o disabilitati tramite il flag `--enable-admission-plugins` di kube-apiserver.

#### Admission Controller Predefiniti

**1. PodSecurity (ex PodSecurityPolicy)**
Controlla le policy di sicurezza dei Pod per garantire che rispettino le best practices di sicurezza. Può limitare:
- L'uso di privilegi (privileged containers)
- L'accesso al filesystem root
- La condivisione di namespace tra container
- L'uso di volumi specifici

**2. ResourceQuota**
Applica limiti di risorse a livello di namespace. Impedisce la creazione di risorse che superano i limiti definiti.

**3. LimitRanger**
Applica limiti di risorse a singoli Pod/Container. Se un Pod non specifica limiti, LimitRanger può applicare valori di default.

**4. DefaultStorageClass**
Assegna automaticamente una StorageClass di default ai PersistentVolumeClaims che non ne specificano una. Utile per semplificare la gestione dello storage.

**5. DefaultTolerationSeconds**
Aggiunge automaticamente tolerations di default ai Pod che non ne specificano, utile per gestire nodi con taint temporanei.

**6. NodeRestriction**
Limita i permessi dei nodi (kubelet) per accedere solo alle risorse che appartengono al nodo stesso. Previene che un nodo compromesso possa accedere a risorse di altri nodi.

**7. ServiceAccount**
Gestisce l'automounting dei ServiceAccount nei Pod. Permette di controllare se i Pod ricevono automaticamente un ServiceAccount e il token associato.

**8. MutatingAdmissionWebhook e ValidatingAdmissionWebhook**
Questi admission controller permettono di estendere Kubernetes con webhook personalizzati:
- **MutatingAdmissionWebhook**: può modificare le richieste prima che vengano validate (es. aggiungere label, modificare spec)
- **ValidatingAdmissionWebhook**: valida le richieste e può rifiutarle se non soddisfano criteri specifici

#### Configurazione degli Admission Controller

Gli admission controller possono essere abilitati/disabilitati avviando kube-apiserver con i flag appropriati:

```bash
kube-apiserver \
  --enable-admission-plugins=PodSecurity,ResourceQuota,LimitRanger \
  --disable-admission-plugins=DefaultStorageClass
```

Per verificare quali admission controller sono attivi:
```bash
kubectl describe --raw /healthz/ready
```

#### Best Practices per gli Admission Controller

- **Abilita sempre**: PodSecurity, ResourceQuota, LimitRanger, NodeRestriction
- **Usa webhook personalizzati**: per policy specifiche dell'organizzazione
- **Testa in staging**: prima di applicare admission controller in produzione
- **Monitora i rifiuti**: logga le richieste rifiutate per debug
- **Documenta le policy**: mantieni una documentazione chiara delle regole applicate

Gli admission controller sono fondamentali per la sicurezza e la governance in Kubernetes, permettendo di applicare policy centralizzate e prevenire configurazioni errate o pericolose.
