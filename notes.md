Introduction
    Problem statement
        (
            le gpu oggi sonno una risorsa critica. AI, datacenter e costi
            non tutti i workload AI sono uguali 
            le gpu sono sottoutilizzate: spreco di risorse
            kubernetes runtime platform principale
            kubernetes alloca queto tipo di risorse in maniera ridamente statica
        )
    Objectives
        (
            aumentare la GPU utilization
            PoC
        )
    Thesis Structure

State of the art
    (altre ricerche che si occupano di migliorare la gpu utilization)    

background and technologies
    gpu sharing
        (descrizione tecnlogie disponibili e motivazione scelta MIG)
    NVIDIA Multi-Instance GPU
        (MIG deep dive)
    gpus in kubernetes
        (device plugin)
    Dynamic Resource Allocation 
        (DRA deep dive)
    Linux GPU Software Stack
    NVIDIA GPU Operator

Architecture
    (definizione dei 3 layer: hardware, allocation e application)

Implementazione
    testbed overview
        (
            descrizione vm, gpu, cluster, configurazioni gpu operator e dashboard garafana
        )
    Use Case 1: Multiple Ollama Instances
        problem statement
            (
                modelli piccoli non saturano la gpu
                Head-of-Line Blocking
            )
        architecture overview
            (
                descrizione claim templates, deploy ollama, modello usato, load generator ecc
            )
        Testing methodologies
            (
                baseline: 1 ollama su intera gpu
                time-sharing
                restringere l'istanza
                istanze multiple                
            )
        Results and Evaluation
            (
                discussione sui numeri
                il right sizing è critico
                DRA no perfomance boost, statico. trovare uno scenario che ne possa benificiare
            )

    Use Case 2: Kueue-Managed Batch Workloads
        problem statement
            (
                la natura dei job si sposa bene con le richieste dinamiche DRA
                priorità garantisce precedenza di allocazione, ma non tempo di esecuzione
                Idea: Service Class
            )
        Kueue background
            (
                cosa è e a cosa serve kueue
                gestione delle priorità in kueue
            )
        architecture overview
            (
                defnizione delle priorità e della classi QoS attraverso la combinazione di deviceClasses e ClaimTemplates e motivazione delle scelte (coservativa vs aggressiva)
                descrizione scenario: job pool 
                baseline gold su M
                baseline gold su L
                baseline mix
                d-flex
            )
        results evaluation

Conclusioni
    Summary
        (
            bisogna scegliere il tipo di sharing in base al workload
            dra porta principalmente vantaggi operativi, non di performance
            mig + dra non è la soluzione perfetta, ma va cucita sulle specifiche necessità per ottenere i massimi vantaggi
        )
    Future work 
        (
            aumatically enforced mig
            AI load prediction and DRA assignment
        )
