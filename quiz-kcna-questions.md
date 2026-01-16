# Quiz KCNA - Kubernetes and Cloud Native Associate - Domande

Questo quiz contiene 60 domande a risposta multipla basate sui contenuti del corso KCNA.

## Sezione 1: Introduzione al Corso (Domande 1-5)

**Domanda 1**
Qual è l'organizzazione che ospita la Cloud Native Computing Foundation (CNCF)?

    A) The Linux Foundation V

    B) Open Source Initiative

    C) Cloud Native Alliance

    D) Kubernetes Foundation

**Domanda 2**
Qual è lo scopo principale degli eventi della Linux Foundation?

    A) Vendere servizi cloud

    B) Incontrare maintainer open source, sviluppatori e tecnologi V

    C) Formare solo i sysadmin

    D) Rilasciare nuove versioni di Kubernetes

**Domanda 3**
Il gruppo Linux Foundation Training & Certification lavora con:

    A) Solo istruttori universitari

    B) Istruttori esperti e sviluppatori open source esperti V

    C) Solo aziene cloud

    D) Solo per principianti

**Domanda 4**
La Cloud Native Computing Foundation è dedicata a rendere il cloud native computing:

    A) Universale e sostenibile V

    B) Esclusivo per le grandi aziende

    C) Gratuito per tutti

    D) Solo per ambienti ibridi

**Domanda 5**
La Linux Foundation è la casa leader mondiale per la collaborazione su:

    A) Software open source, hardware, standard e dati V

    B) Solo software proprietario

    C) Solo hardware

    D) Solo standard

## Sezione 2: Architettura Cloud Native (Domande 6-15)

**Domanda 6**
Secondo la CNCF, quali tecnologie consentono di creare ed eseguire applicazioni scalabili in ambienti moderni?

    A) Solo container

    B) Container, service mesh, microservizi, infrastruttura immutabile e API dichiarative V

    C) Solo microservizi

    D) Solo API dichiarative

**Domanda 7**
Qual è una caratteristica dell'architettura monolitica?

    A) Facile da gestire la complessità

    B) Singola base di codice fornita come un singolo file binario V

    C) Facile da scalare tra più team

    D) Ogni team ha proprietà di diverse funzioni

**Domanda 8**
Qual è un vantaggio dell'architettura a microservizi?

    A) Difficile da gestire la complessità

    B) Difficile da mantenere le applicazioni

    C) Facile operare e scalare le applicazioni individualmente V

    D) Scalare inefficientemente

**Domanda 9**
Quale di queste è una caratteristica dell'architettura cloud native?

    A) Basso livello di automazione

    B) Self healing V

    C) Non scalabile

    D) Costi non efficienti

**Domanda 10**
Cos'è l'autoscaling?

    A) Regolazione statica delle risorse

    B) Regolazione dinamica delle risorse in base alla domanda V

    C) Solo scaling verticale

    D) Solo scaling orizzontale

**Domanda 11**
Quale tipo di scaling descrive il cambiamento nelle dimensioni dell'hardware sottostante?

    A) Horizontal Scaling

    B) Vertical Scaling V

    C) Diagonal Scaling

    D) Cloud Scaling

**Domanda 12**
Parlando di autoscaling, di solito ci si riferisce a:

    A) Vertical Scaling

    B) Horizontal Scaling V

    C) Scaling diagonale

    D) Scaling statico

**Domanda 13**
Cos'è il serverless computing?

    A) Un server fisico dedicato

    B) Un approccio che solleva gli sviluppatori da compiti di provisioning e configurazione V

    C) Solo virtualizzazione

    D) Solo container

**Domanda 14**
Qual è lo scopo del progetto CloudEvents?

    A) Fornire una specifica su come strutturare i dati degli eventi V

    B) Creare server fisici

    C) Gestire i container

    D) Monitorare le applicazioni

**Domanda 15**
Quali standard fornisce l'Open Container Initiative (OCI)?

    A) Solo image-spec

    B) Image-spec, runtime-spec e distribution-spec V

    C) Solo runtime-spec

    D) Solo distribution-spec

## Sezione 3: Orchestrazione dei Container (Domande 16-25)

**Domanda 16**
Qual è uno dei primi antenati delle moderne tecnologie dei container?

    A) Docker

    B) chroot V

    C) Kubernetes

    D) Podman

**Domanda 17**
Cosa isolano i namespaces nel kernel Linux?

    A) Solo la rete

    B) Varie risorse come rete, processi, mount, IPC, utenti, ecc. V

    C) Solo la CPU

    D) Solo la memoria

**Domanda 18**
Cosa sono i cgroups?

    A) Gruppi di container

    B) Gruppi gerarchici di processi per assegnare risorse come memoria e CPU V

    C) Gruppi di rete

    D) Gruppi di utenti

**Domanda 19**
Come differiscono i container dalle macchine virtuali?

    A) I container emulano un'intera macchina

    B) I container condividono il kernel della macchina host e sono processi isolati V

    C) I container hanno più sovraccarico

    D) I container si avviano più lentamente

**Domanda 20**
Qual è l'implementazione di riferimento del runtime del container mantenuta da OCI?

    A) Docker

    B) runC V

    C) containerd

    D) CRI-O

**Domanda 21**
Quali sono le 4C della sicurezza Cloud Native?

    A) Cloud, Container, Cluster, Codice V

    B) Computer, Container, Cloud, Codice

    C) Container, Cluster, Cloud, Codice

    D) Cloud, Container, Cluster, Company

**Domanda 22**
Qual è uno dei maggiori rischi per la sicurezza dei container?

    A) Esecuzione di processi con troppi privilegi V

    B) Esecuzione di processi senza privilegi

    C) Isolamento eccessivo

    D) Condivisione del kernel

**Domanda 23**
Quali sono i due componenti principali di un sistema di orchestrazione dei container?

    A) Control plane e worker node V

    B) Solo control plane

    C) Solo worker node

    D) Client e server

**Domanda 24**
Qual è lo scopo della Service Discovery?

    A) Trovare altri servizi nella rete e richiedere informazioni su di essi V

    B) Creare servizi

    C) Eliminare servizi

    D) Monitorare servizi

**Domanda 25**
Cos'è una service mesh?

    A) Un tipo di container

    B) Un software che aggiunge un server proxy a ogni container per gestire il traffico di rete V

    C) Un database

    D) Un sistema di storage

## Sezione 4: Fondamenti di Kubernetes (Domande 26-35)

**Domanda 26**
Quali tipi di nodi compongono un cluster Kubernetes?

    A) Solo control plane node

    B) Solo worker node

    C) Control plane node e worker node V

    D) Solo etcd node

**Domanda 27**
Qual è il componente del control plane che gestisce lo stato del cluster?

    A) kube-apiserver

    B) etcd V

    C) kube-scheduler

    D) kube-controller-manager

**Domanda 28**
Qual è il componente del control plane che sceglie il worker node adatto per un pod?

    A) kube-apiserver

    B) etcd

    C) kube-scheduler V

    D) kube-controller-manager

**Domanda 29**
Qual è il componente del worker node che parla con l'api-server e il container runtime?

    A) container runtime

    B) kubelet V

    C) kube-proxy

    D) etcd

**Domanda 30**
Cosa fa kube-proxy?

    A) Esegue container

    B) Gestisce la comunicazione interna ed esterna del cluster V

    C) Memorizza lo stato del cluster

    D) Schedula i pod

**Domanda 31**
Cosa succede alle applicazioni già avviate su un worker node se il control plane non è disponibile?

    A) Si fermano immediatamente

    B) Continuano a girare V

    C) Vengono riavviate automaticamente

    D) Vengono eliminate

**Domanda 32**
Cos'è un namespace di Kubernetes?

    A) Un namespace del kernel

    B) Una divisione del cluster in cluster virtuali per multi-tenancy V

    C) Un isolamento di container

    D) Un tipo di pod

**Domanda 33**
Qual è l'API di Kubernetes?

    A) Un'interfaccia SOAP

    B) Un'interfaccia RESTful esposta su HTTPS V

    C) Un'interfaccia gRPC

    D) Un'interfaccia FTP

**Domanda 34**
Quali sono le tre fasi prima che una richiesta venga elaborata da Kubernetes?

    A) Autenticazione, Autorizzazione, Admission Control V

    B) Autenticazione, Autorizzazione, Criptazione

    C) Autorizzazione, Admission Control, Criptazione

    D) Autenticazione, Criptazione, Admission Control

**Domanda 35**
Qual è la più piccola unità di calcolo in Kubernetes?

    A) Container

    B) Pod V

    C) Deployment

    D) Service

## Sezione 5: Lavorare con Kubernetes (Domande 36-45)

**Domanda 36**
Quali campi sono richiesti per un oggetto Kubernetes?

    A) apiVersion, kind, metadata, spec

    B) Solo name e kind

    C) Solo metadata e spec

    D) apiVersion e kind

**Domanda 37**
Cos'è Helm?

    A) Un container runtime

    B) Un package manager per Kubernetes

    C) Un sistema di storage

    D) Un sistema di monitoraggio

**Domanda 38**
Cosa descrive un Pod?

    A) Un'unità di uno o più container che condividono namespaces e cgroups

    B) Un singolo container

    C) Un deployment

    D) Un service

**Domanda 39**
Tutti i container all'interno di un Pod condividono:

    A) Solo il filesystem

    B) Un indirizzo IP e possono condividere tramite il filesystem

    C) Solo la CPU

    D) Solo la memoria

**Domanda 40**
Cos'è un sidecar container?

    A) Un container principale

    B) Un container che supporta l'applicazione principale

    C) Un container di database

    D) Un container di rete

**Domanda 41**
Quali sono le fasi del ciclo di vita di un Pod?

    A) Pending, Running, Succeeded, Failed, Unknown

    B) Pending, Running, Stopped, Failed

    C) Created, Running, Stopped, Deleted

    D) Pending, Running, Paused, Failed

**Domanda 42**
Qual è lo scopo di un ReplicaSet?

    A) Eseguire applicazioni stateful

    B) Assicurare che un numero desiderato di Pod sia in esecuzione

    C) Eseguire job one-shot

    D) Gestire i volumi

**Domanda 43**
Qual è lo scopo di un Deployment?

    A) Eseguire applicazioni stateful

    B) Gestire il ciclo di vita completo dell'applicazione gestendo multiple ReplicaSet

    C) Eseguire job one-shot

    D) Gestire i volumi

**Domanda 44**
Qual è lo scopo di un StatefulSet?

    A) Eseguire applicazioni stateless

    B) Eseguire applicazioni stateful come database

    C) Eseguire job one-shot

    D) Gestire i volumi

**Domanda 45**
Qual è lo scopo di un DaemonSet?

    A) Eseguire un Pod su tutti i node del cluster per carichi di lavoro infrastrutturali

    B) Eseguire un Pod solo su un node

    C) Eseguire job one-shot

    D) Gestire i volumi

## Sezione 6: Consegna delle Applicazioni Cloud Native (Domande 46-52)

**Domanda 46**
Cos'è la Continuous Integration?

    A) Automatizzare il deployment del software pre-costruito

    B) Costruire e testare permanentemente il codice scritto

    C) Solo distribuire in produzione

    D) Solo monitorare

**Domanda 47**
Cos'è la Continuous Delivery?

    A) Costruire e testare permanentemente il codice scritto

    B) Automatizzare il deployment del software pre-costruito

    C) Solo scrivere codice

    D) Solo monitorare

**Domanda 48**
Quali sono gli approcci per implementare le modifiche in una pipeline CI/CD?

    A) Push-based e pull-based

    B) Solo push-based

    C) Solo pull-based

    D) Manual-based

**Domanda 49**
Cos'è GitOps?

    A) Un approccio che usa Git per descrivere la configurazione, rete, policy o sicurezza

    B) Un sistema di controllo versione

    C) Un container runtime

    D) Un sistema di monitoraggio

**Domanda 50**
Quali sono due esempi di framework GitOps pull-based?

    A) Flux e ArgoCD

    B) Jenkins e GitLab

    C) Spinnaker e Tekton

    D) Docker e Podman

**Domanda 51**
Come funziona Kubernetes per quanto riguarda GitOps?

    A) Usa un approccio push-based

    B) Monitora un database per le modifiche e le applica se non corrisponde allo stato desiderato

    C) Non usa Git

    D) Solo manualmente

**Domanda 52**
Quali strumenti CI/CD sono popolari per cloud native?

    A) Solo Jenkins

    B) Spinnaker, GitLab, Jenkins, Jenkins X, Tekton CD, ArgoCD

    C) Solo Docker

    D) Solo Kubernetes

## Sezione 7: Osservabilità Cloud Native (Domande 53-60)

**Domanda 53**
Cos'è l'osservabilità?

    A) Solo raccogliere dati

    B) Consentire l'analisi dei dati raccolti per manipolare il comportamento del sistema

    C) Solo monitorare

    D) Solo loggare

**Domanda 54**
Quali sono le tre categorie di dati di telemetria?

    A) Logs, Metrics, Traces

    B) Logs, Events, Alerts

    C) Metrics, Events, Traces

    D) Logs, Metrics, Events

**Domanda 55**
Quali stream di I/O possono essere utilizzati per emettere log da un container?

    A) stdin e stdout

    B) stdout e stderr

    C) stdin e stderr

    D) Solo stdout

**Domanda 56**
Come si visualizzano i log di un pod in Kubernetes?

    A) kubectl logs [nome pod]

    B) docker logs [nome container]

    C) kubectl get logs [nome pod]

    D) kubectl describe logs [nome pod]

**Domanda 57**
Quali sono i metodi per spedire (ship) i log?

    A) Logging a livello di Node, Logging tramite sidecar container, Logging a livello di applicazione

    B) Solo logging a livello di Node

    C) Solo logging a livello di applicazione

    D) Solo logging tramite sidecar

**Domanda 58**
Quali sono le 4 metriche principali di Prometheus?

    A) Counter, Gauge, Histogram, Summary

    B) Counter, Gauge, Timer, Summary

    C) Counter, Timer, Histogram, Summary

    D) Gauge, Timer, Histogram, Summary

**Domanda 59**
Qual è il linguaggio di query di Prometheus?

    A) SQL

    B) PromQL

    C) GraphQL

    D) NoSQL

**Domanda 60**
Cos'è OpenTelemetry?

    A) Un sistema di logging

    B) Un insieme di API, SDK e strumenti per integrare metriche, protocolli e tracce

    C) Un container runtime

    D) Un sistema di storage