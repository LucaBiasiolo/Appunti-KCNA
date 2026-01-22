# 06. Consegna delle Applicazioni Cloud Native (Cloud Native Application Delivery)
## Fondamenti della Consegna delle Applicazioni
Il modo migliore per gestire il codice sorgente è un sistema di controllo della versione (Version Control System).
Nel 2005 Linus Torvalds ha creato *Git*, un sistema decentralizzato che può essere utilizzato per tracciare le modifiche nel codice sorgente.
Con il codice sorgente sotto controllo, il passo successivo prima di consegnare l'applicazione è costruirla (build).
Per garantire l'alta qualità dell'applicazione, il passo successivo dovrebbe essere un test esteso e automatico dell'app per assicurarsi che tutte le funzionalità siano ancora al loro posto dopo che qualcuno ha apportato modifiche.
L'ultimo passaggio è la consegna dell'applicazione alla piattaforma su cui dovrebbe essere eseguita.

## CI/CD
***Continuous Integration*** è la prima parte del processo e ***descrive la costruzione e il test permanenti del codice scritto***. L'alta automazione e l'uso del controllo di versione consentono a più sviluppatori e team di lavorare sulla stessa base di codice.
***Continuous Delivery*** è la seconda parte del processo e ***automatizza il deployment del software pre-costruito***. In ambienti cloud, vedrai spesso che il software viene distribuito in ambienti di Sviluppo (Development) o Staging, prima di essere rilasciato e consegnato a un sistema di produzione.
Per automatizzare l'intero flusso di lavoro, puoi usare una **pipeline CI/CD**, che in realtà non è altro che la forma scritta (scriptata) di tutti i passaggi coinvolti, in esecuzione su un server o anche in un container.
Strumenti CI/CD popolari includono:
- [Spinnaker](https://spinnaker.io/)
- [GitLab](https://gitlab.com/)
- [Jenkins](https://www.jenkins.io/)
- [Jenkins X](https://jenkins-x.io/)
- [Tekton CD](https://github.com/tektoncd/pipeline)
- [ArgoCD](https://argoproj.github.io/)


## GitOps
L'Infrastructure as Code è stata una vera rivoluzione nell'aumentare la qualità e la velocità della fornitura di infrastrutture, e funziona così bene che oggi configurazione, rete, policy o sicurezza possono essere descritte come codice, e spesso risiedono persino nello stesso repository del software.
Esistono due diversi approcci su come una pipeline CI/CD può implementare le modifiche che desideri apportare:
- **Push-based**
La pipeline viene avviata ed esegue strumenti che apportano le modifiche nella piattaforma. Le modifiche possono essere attivate da un commit o da una merge request.
- **Pull-based**
Un agente monitora il repository Git per eventuali modifiche e confronta la definizione nel repository con lo stato di esecuzione effettivo. Se vengono rilevate modifiche, l'agente applica le modifiche all'infrastruttura.

Due esempi di popolari framework GitOps che utilizzano l'approccio pull-based sono [Flux](https://fluxcd.io/) and [ArgoCD](https://argo-cd.readthedocs.io/).
ArgoCD è implementato come un controller Kubernetes, mentre Flux è costruito con il GitOps Toolkit, un set di API e controller che possono essere utilizzati per estendere Flux o persino costruire una piattaforma di consegna personalizzata.
***Kubernetes utilizza un'idea simile all'approccio pull-based: viene monitorato un database per le modifiche e le modifiche vengono applicate allo stato di esecuzione se non corrisponde allo stato desiderato.***
