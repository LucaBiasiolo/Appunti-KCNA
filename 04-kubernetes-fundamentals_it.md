# 04. Fondamenti di Kubernetes (Kubernetes Fundamentals)
## Architettura di Kubernetes (Kubernetes Architecture)
I Cluster Kubernetes sono composti da due diversi tipi di server Node che costituiscono un Cluster:
- **Control plane node(s)**
Questi sono il cervello dell'operazione. I Control plane node contengono vari componenti che gestiscono il Cluster e controllano vari compiti come deployment, scheduling e self-healing dei carichi di lavoro containerizzati.
- **Worker nodes**
I Worker node sono il luogo in cui le applicazioni vengono eseguite nel Cluster. Questo è l'unico compito dei Worker node e non hanno ulteriori logiche implementate. Il loro comportamento, come ad esempio se debbano avviare un container, è completamente controllato dal Control plane node.

I Control plane node tipicamente ospitano i seguenti servizi:
- **kube-apiserver** tutti gli altri componenti interagiscono con l'api-server e qui è dove gli utenti accedono al Cluster
- **etcd** un database che contiene lo stato del Cluster
- **kube-scheduler** quando deve essere schedulato un nuovo carico di lavoro, il kube-scheduler sceglie un Worker node adatto, basandosi su diverse proprietà come CPU e memoria
- **kube-controller-manager** contiene diversi cicli di controllo non terminanti che gestiscono lo stato del Cluster
- **cloud-controller-manager** (opzionale) può essere utilizzato per interagire con le API dei fornitori di cloud, per creare risorse esterne come load balancer, storage o gruppi di sicurezza

Componenti dei Worker node:
- **container runtime** è responsabile dell'esecuzione dei container sul Worker node
- **kubelet** è un piccolo agente che gira su ogni Worker node nel Cluster, parla con l'api-server e il container runtime per gestire la fase finale dell'avvio dei container
- **kube-proxy** è un proxy di rete che gestisce la comunicazione interna ed esterna del Cluster, cercando di fare affidamento sulle capacità di networking del sistema operativo sottostante se possibile

***È importante notare che questo design rende possibile che le applicazioni già avviate su un Worker node continuino a girare anche se il control plane non è disponibile. Anche se molte funzionalità importanti come la scalabilità, lo scheduling di nuove applicazioni, ecc., non saranno possibili mentre il control plane è offline.***
Kubernetes ha anche il concetto di *namespaces*, da non confondere con i namespace del kernel che vengono utilizzati per isolare i container. **Un Namespace di Kubernetes può essere utilizzato per dividere un Cluster in *molteplici cluster virtuali***, che possono essere utilizzati per la multi-tenancy quando più team condividono un Cluster.
![image](..files\image.png)

## Configurazione di Kubernetes (Kubernetes Setup)
Creare un "cluster" di test può essere molto semplice con:
- [Minikube](https://minikube.sigs.k8s.io/docs/) - supporta macOS, Linux e Windows
- [kind](https://kind.sigs.k8s.io/)
- [MicroK8s](https://microk8s.io/)

Configura un Cluster di livello enterprise sul tuo hardware o su macchine virtuali
- [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)
- [kops](https://github.com/kubernetes/kops)
- [kubespray](https://github.com/kubernetes-sigs/kubespray)

Kubernetes pacchettizzato in una distribuzione:
- [Rancher](https://rancher.com/)
- [k3s](https://k3s.io/)
- [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift)
- [VMWare Tanzu](https://tanzu.vmware.com/tanzu)

Provider Cloud:
- [Amazon (EKS)](https://aws.amazon.com/eks/)
- [Google (GKE)](https://cloud.google.com/kubernetes-engine)
- [Microsoft (AKS)](https://azure.microsoft.com/en-us/services/kubernetes-service)
- [DigitalOcean (DOKS)](https://www.digitalocean.com/products/kubernetes/)


## API di Kubernetes (Kubernetes API)
Senza di essa, la comunicazione con il Cluster non è possibile, ogni utente e ogni componente del Cluster stesso ha bisogno dell'api-server.
**L'API di Kubernetes è implementata come un'interfaccia RESTful esposta su HTTPS.**
Prima che una richiesta venga elaborata da Kubernetes, deve passare attraverso tre fasi:
- **Autenticazione (Authentication)**
Il richiedente deve presentare un mezzo di identità per autenticarsi contro l'API. Comunemente fatto con un certificato firmato digitalmente ([X.509](https://en.wikipedia.org/wiki/X.509)) o con un sistema di gestione dell'identità esterno. Gli utenti di Kubernetes sono *sempre* gestiti esternamente. I [Service Accounts](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/) possono essere utilizzati per autenticare utenti tecnici.
- **Autorizzazione (Authorization)**
Viene deciso cosa il richiedente è autorizzato a fare. In Kubernetes questo può essere fatto con il [Role Based Access Control (RBAC)](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).
- **Admission Control**
Nell'ultimo passaggio, gli [admission controllers](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/) possono essere utilizzati per modificare o convalidare la richiesta. Ad esempio, se un utente tenta di utilizzare un'immagine di container da un registro inaffidabile, un admission controller potrebbe bloccare questa richiesta. Strumenti come l'[Open Policy Agent](https://www.openpolicyagent.org/) possono essere utilizzati per gestire l'admission control esternamente.
![image](..files\image_3.png)


## Esecuzione di Container su Kubernetes
In Kubernetes, invece di avviare direttamente i container, si **definiscono i Pod come la più piccola unità di calcolo** e Kubernetes la traduce in un container in esecuzione.
![image](..files\image_a.png)
Container Runtime:
- **containerd** è un'implementazione leggera e performante per eseguire container
- **CRI-O** è stato creato da Red Hat
- **Docker** lo standard per molto tempo, ma mai realmente creato per l'orchestrazione dei container (rimosso da Kubernetes 1.24)

containerd e CRI-O cercano di risolvere il problema di sicurezza che deriva dalla condivisione del kernel tra più container. Gli strumenti più comuni al momento sono:
- [gvisor](https://github.com/google/gvisor)
Realizzato da Google, fornisce un kernel dell'applicazione che si trova tra il processo containerizzato e il kernel dell'host.
- [Kata Containers](https://katacontainers.io/)
Un runtime sicuro che fornisce una macchina virtuale leggera, ma si comporta come un container.


## Networking
I container devono comunicare attraverso molti Node. Kubernetes distingue tra quattro diversi problemi di networking che devono essere risolti:
1. **Comunicazione Container-to-Container**
Questo può essere risolto dal concetto di Pod, come vedremo più avanti.
2. **Comunicazione Pod-to-Pod**
Questo può essere risolto con una rete overlay.
3. **Comunicazione Pod-to-Service**
È implementata dal kube-proxy e dal packet filter sul Node.
4. **Comunicazione External-to-Service**
È implementata dal kube-proxy e dal packet filter sul Node.

Esistono diversi modi per implementare il networking in Kubernetes, ma anche tre requisiti importanti:
- Tutti i Pod possono comunicare tra loro attraverso i Node.
- Tutti i Node possono comunicare con tutti i Pod.
- Nessuna Network Address Translation (NAT).

In Kubernetes, **ogni Pod riceve il proprio indirizzo IP**, quindi non è necessaria alcuna configurazione manuale. Inoltre, la maggior parte delle **configurazioni di Kubernetes include un add-on per il server DNS chiamato [core-dns](https://kubernetes.io/docs/tasks/administer-cluster/coredns/)**, che può fornire service discovery e risoluzione dei nomi all'interno del Cluster.
Ogni Pod può comunicare con altri Pod sul Cluster Kubernetes, tuttavia se desideri controllare il flusso di traffico a livello di indirizzo IP o porta, devi utilizzare le Network Policies.

## Scheduling
Descrive il processo di scelta automatica del Node (Worker) giusto per eseguire un carico di lavoro containerizzato.
Il kube-scheduler è il componente che prende la decisione di scheduling, ma non è responsabile dell'avvio effettivo del carico di lavoro (verrà avviato dal kubelet e dal container runtime). Il processo di scheduling in Kubernetes inizia sempre quando viene creato un nuovo oggetto Pod.
Un utente deve fornire informazioni sui requisiti dell'applicazione, comprese le richieste di CPU e memoria e le proprietà di un Node.
Lo scheduler utilizzerà queste informazioni per filtrare tutti i Node che soddisfano questi requisiti. Se più Node soddisfano i requisiti allo stesso modo, Kubernetes pianificherà il Pod sul Node con la minor quantità di Pod.
Se i Worker node non hanno risorse sufficienti per eseguire la tua applicazione, lo scheduler riproverà a trovare un Node appropriato fino a quando lo stato non potrà essere stabilito.
