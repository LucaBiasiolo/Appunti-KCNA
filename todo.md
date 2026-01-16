# 📋 TODO LIST - PREPARAZIONE ESAME KCNA

## 🎯 Obiettivo: 60 domande in 90 minuti (Target: 55/60 corrette)

---

## 📚 FASE 1: COMPRENSIONE FONDAMENTALE (Giorni 1-2)

### Giorno 1: Lettura Attiva
- [ ] **Capitolo 1: Introduzione al Corso** (30 min)
  - [ ] Leggere attentamente 01-course-introduction_it.md
  - [ ] Evidenziare: Linux Foundation, CNCF, eventi, formazione
  - [ ] Creare mappa mentale: Organizzazioni e certificazioni

- [ ] **Capitolo 2: Architettura Cloud Native** (90 min)
  - [ ] Leggere 02-cloud-native-architecture_it.md
  - [ ] Evidenziare: Definizione CNCF, microservizi vs monolitico
  - [ ] Creare mappa mentale: Caratteristiche cloud native, autoscaling, serverless
  - [ ] Memorizzare: Standard OCI (image-spec, runtime-spec, distribution-spec)
  - [ ] Memorizzare: CNI, CRI, CSI, SMI
  - [ ] Memorizzare: Ruoli cloud native (Cloud Architect, DevOps, SRE)
  - [ ] Memorizzare: SLI, SLO, SLA
  - [ ] Memorizzare: CNCF TOC e criteri di graduation

- [ ] **Capitolo 3: Orchestrazione dei Container** (90 min)
  - [ ] Leggere 03-container-orchestration_it.md
  - [ ] Evidenziare: Container vs VM, namespaces, cgroups
  - [ ] Creare mappa mentale: Sicurezza 4C, Service Mesh, Storage
  - [ ] Memorizzare: 8 namespaces (pid, net, mnt, ipc, user, utime, cgroup, time)
  - [ ] Memorizzare: Runtime (runC, containerd, CRI-O)
  - [ ] Memorizzare: Data plane vs Control plane in Service Mesh

### Giorno 2: Lettura Continua
- [ ] **Capitolo 4: Fondamenti di Kubernetes** (90 min)
  - [ ] Leggere 04-kubernetes-fundamentals_it.md
  - [ ] Evidenziare: Architettura, componenti, API
  - [ ] Creare mappa mentale: Control plane vs Worker nodes
  - [ ] Memorizzare: kube-apiserver, etcd, kube-scheduler, kube-controller-manager
  - [ ] Memorizzare: kubelet, kube-proxy
  - [ ] Memorizzare: 3 fasi API (Autenticazione, Autorizzazione, Admission Control)
  - [ ] Memorizzare: 4 problemi di networking Kubernetes

- [ ] **Capitolo 5: Lavorare con Kubernetes** (90 min)
  - [ ] Leggere 05-working-with-kubernetes_it.md
  - [ ] Evidenziare: Oggetti Kubernetes, Pod, workload objects
  - [ ] Creare mappa mentale: Deployment, StatefulSet, DaemonSet, Job, CronJob
  - [ ] Memorizzare: Tipi di Service (ClusterIP, NodePort, LoadBalancer, ExternalName, Headless)
  - [ ] Memorizzare: Autoscaling (HPA, VPA, Cluster Autoscaler)
  - [ ] Memorizzare: Scheduling (nodeSelector, affinity, taint/toleration)
  - [ ] Memorizzare: Sicurezza (RBAC, admission controllers)

- [ ] **Capitolo 6: Consegna Applicazioni Cloud Native** (45 min)
  - [ ] Leggere 06-cloud-native-application-delivery_it.md
  - [ ] Evidenziare: CI/CD, GitOps
  - [ ] Creare mappa mentale: Push-based vs Pull-based
  - [ ] Memorizzare: Flux, ArgoCD

- [ ] **Capitolo 7: Osservabilità Cloud Native** (45 min)
  - [ ] Leggere 07-cloud-native-observability_it.md
  - [ ] Evidenziare: Telemetria, Logging, Prometheus, Tracing
  - [ ] Creare mappa mentale: Logs, Metrics, Traces
  - [ ] Memorizzare: Metriche Prometheus (Counter, Gauge, Histogram, Summary)
  - [ ] Memorizzare: PromQL

---

## 🎮 FASE 2: PRATICA CON QUIZ (Giorni 3-4)

### Giorno 3: Primo Round
- [ ] **Quiz Iniziale** (90 min)
  - [ ] Fare il quiz completo senza guardare le risposte
  - [ ] Cronometrare il tempo (target: <90 minuti)
  - [ ] Registrare il punteggio iniziale
  - [ ] Identificare le domande più difficili

- [ ] **Analisi Errori** (60 min)
  - [ ] Per ogni risposta sbagliata:
    - [ ] Identificare il capitolo corrispondente
    - [ ] Rileggere la sezione specifica (non tutto il capitolo)
    - [ ] Creare una nota con la correzione
    - [ ] Aggiungere la domanda a una lista di ripasso

- [ ] **Ripasso Mirato** (60 min)
  - [ ] Rileggere i capitoli con più errori
  - [ ] Creare flashcard per concetti mancanti
  - [ ] Praticare con esempi pratici

### Giorno 4: Secondo Round
- [ ] **Quiz Ripetuto** (90 min)
  - [ ] Rifare il quiz cronometrato
  - [ ] Target: 50/60 corrette (83%)
  - [ ] Analizzare nuovi errori

- [ ] **Quiz Finale** (90 min)
  - [ ] Ultimo quiz cronometrato
  - [ ] Target: 55/60 corrette (91.7%)
  - [ ] Se sotto il target, rivedere aree di debolezza

---

## 🔍 FASE 3: APPROFONDIMENTO (Giorni 5-6)

### Giorno 5: Concetti Complessi
- [ ] **Service Mesh** (60 min)
  - [ ] Studiare dettagli su Istio e Linkerd
  - [ ] Capire Data plane vs Control plane
  - [ ] Esempi pratici di configurazione
  - [ ] Creare diagramma di flusso

- [ ] **RBAC e Sicurezza** (60 min)
  - [ ] Studiare Role, ClusterRole, RoleBinding, ClusterRoleBinding
  - [ ] Esempi pratici di permessi
  - [ ] Memorizzare verbi (create, read, update, delete)
  - [ ] Studiare PodSecurityPolicy (deprecato ma rilevante)
  - [ ] Studiare PodSecurityStandards

- [ ] **Autoscaling** (60 min)
  - [ ] Studiare HPA, VPA, Cluster Autoscaler in dettaglio
  - [ ] Capire metriche e soglie
  - [ ] Esempi pratici di configurazione
  - [ ] Studiare KEDA per event-driven scaling

### Giorno 6: GitOps e Monitoring
- [ ] **GitOps** (60 min)
  - [ ] Studiare differenze push-based vs pull-based
  - [ ] Configurazione Flux vs ArgoCD
  - [ ] Esempi pratici di repository GitOps
  - [ ] Studiare best practices

- [ ] **Prometheus e Monitoring** (60 min)
  - [ ] Studiare metriche di Prometheus in dettaglio
  - [ ] Praticare PromQL (query language)
  - [ ] Studiare Grafana per dashboard
  - [ ] Studiare Alertmanager per avvisi
  - [ ] Studiare kube-state-metrics e cAdvisor

- [ ] **Kubernetes Objects Specifici** (60 min)
  - [ ] Studiare initContainers con esempi
  - [ ] Studiare ResourceQuota e LimitRange
  - [ ] Studiare ReadinessProbe e LivenessProbe
  - [ ] Studiare RollingUpdate vs Recreate strategy
  - [ ] Studiare ConfigMap e Secret con esempi pratici

---

## 🎯 FASE 4: SIMULAZIONE ESAME (Giorno 7)

### Simulazione Completa
- [ ] **Simulazione Esame** (90 min)
  - [ ] Ambiente silenzioso, senza interruzioni
  - [ ] Cronometro attivo per 90 minuti
  - [ ] 60 domande a risposta multipla
  - [ ] Target: 55/60 corrette (91.7%)

- [ ] **Analisi Post-Simulazione** (60 min)
  - [ ] Calcolare punteggio e percentuale
  - [ ] Identificare aree di debolezza
  - [ ] Creare lista di ripasso finale
  - [ ] Rivedere domande più difficili

- [ ] **Gestione Tempo** (30 min)
  - [ ] Praticare 1.5 minuti per domanda
  - [ ] Strategia: Leggi → Rispondi → Rivedi (se rimane tempo)
  - [ ] Esercitarsi a saltare domande difficili e tornarci dopo

---

## 📝 FASE 5: REVISIONE FINALE (Giorno 8)

### Ripasso Veloce
- [ ] **Ripasso Rapido Tutti i Capitoli** (120 min)
  - [ ] Capitolo 1: 10 min
  - [ ] Capitolo 2: 20 min
  - [ ] Capitolo 3: 20 min
  - [ ] Capitolo 4: 20 min
  - [ ] Capitolo 5: 20 min
  - [ ] Capitolo 6: 15 min
  - [ ] Capitolo 7: 15 min

- [ ] **Focus su Aree di Debolezza** (60 min)
  - [ ] Rivedere le domande più difficili del quiz
  - [ ] Ripassare concetti mancanti
  - [ ] Praticare terminologia specifica

- [ ] **Checklist Pre-Exam** (30 min)
  - [ ] Conosci la definizione CNCF di "cloud native"
  - [ ] Distingui tra vertical e horizontal scaling
  - [ ] Sai cosa fa ogni componente di Kubernetes
  - [ ] Conosci i 3 tipi di telemetria
  - [ ] Distingui tra push-based e pull-based GitOps
  - [ ] Conosci le fasi del Pod
  - [ ] Sai cosa sono i 4C della sicurezza
  - [ ] Conosci i tipi di Service Kubernetes
  - [ ] Distingui tra Deployment, StatefulSet, DaemonSet
  - [ ] Conosci le metriche di Prometheus

---

## 🎯 CHECKLIST FINALE PRE-ESAME

### Conoscenza Teorica
- [ ] Definizione CNCF di "cloud native"
- [ ] Differenza microservizi vs monolitico
- [ ] Caratteristiche cloud native (6 punti)
- [ ] Autoscaling (vertical vs horizontal)
- [ ] Serverless e FaaS
- [ ] Standard OCI (3 spec)
- [ ] CNI, CRI, CSI, SMI
- [ ] Ruoli cloud native (Cloud Architect, DevOps, SRE)
- [ ] SLI, SLO, SLA
- [ ] CNCF TOC e criteri di graduation

### Kubernetes
- [ ] Architettura (control plane vs worker nodes)
- [ ] Componenti principali (kube-apiserver, etcd, kube-scheduler, kube-controller-manager, kubelet, kube-proxy)
- [ ] API (autenticazione, autorizzazione, admission control)
- [ ] Namespaces Kubernetes vs kernel
- [ ] Pod lifecycle (Pending, Running, Succeeded, Failed, Unknown)
- [ ] Workload objects (Deployment, StatefulSet, DaemonSet, Job, CronJob)
- [ ] Service types (ClusterIP, NodePort, LoadBalancer, ExternalName, Headless)
- [ ] Ingress e NetworkPolicy
- [ ] Storage (PV, PVC, StorageClass)
- [ ] ConfigMap e Secrets
- [ ] Autoscaling (HPA, VPA, Cluster Autoscaler)
- [ ] Scheduling (nodeSelector, affinity, taint/toleration)
- [ ] Sicurezza (RBAC, admission controllers)

### DevOps e Delivery
- [ ] CI/CD concetti
- [ ] GitOps (push-based vs pull-based)
- [ ] Flux e ArgoCD

### Observability
- [ ] Telemetria (Logs, Metrics, Traces)
- [ ] Prometheus metriche (Counter, Gauge, Histogram, Summary)
- [ ] PromQL
- [ ] Grafana e Alertmanager
- [ ] OpenTelemetry e Jaeger

### Comandi kubectl Essenziali
- [ ] kubectl get pods,deployments,services
- [ ] kubectl describe pod <nome>
- [ ] kubectl logs <pod>
- [ ] kubectl apply -f <file.yaml>
- [ ] kubectl delete -f <file.yaml>
- [ ] kubectl get nodes
- [ ] kubectl get namespaces

---

## 📊 METRICHE DI SUCCESSO

- [ ] Quiz iniziale: Registrare punteggio
- [ ] Quiz finale: Target 55/60 (91.7%)
- [ ] Simulazione esame: Target 55/60 in 90 minuti
- [ ] Tempo per domanda: <1.5 minuti
- [ ] Checklist pre-exam: 100% completato

---

## 🎯 CONSIGLI FINALI

1. **Non memorizzare, comprendere**: Il KCNA testa la comprensione concettuale
2. **Pratica con quiz**: Il quiz è il tuo miglior strumento di apprendimento
3. **Gestione tempo**: 1.5 minuti per domanda, salta e torna indietro se necessario
4. **Riposo**: Il giorno prima dell'esame, riposo mentale, solo rivedere note
5. **Confidenza**: Con il materiale completo e la pratica, sarai pronto!

**Buona fortuna per l'esame KCNA!** 🚀