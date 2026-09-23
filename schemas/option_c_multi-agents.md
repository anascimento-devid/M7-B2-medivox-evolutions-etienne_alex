# Option C — Multi-agents

```mermaid
flowchart LR
    subgraph SIH["SI hospitalier (source)"]
        DPI[("DPI : données structurées")]
        CR[("Comptes-rendus<br/>(texte libre)")]
    end

    subgraph PF["Plateforme MediVox (équipe data)"]
        ORCH["Agent orchestrateur"]
        CTRL["Agent contrôle des entrées"]
        E{"Données exploitables ?"}

        subgraph AG["Agents spécialisés"]
            ADS{{"Agent données structurées"}}
            AEC{{"Agent extraction clinique"}}
        end

        CONS["Consolidation des variables"]
        VAL["Agent validation"]
        K{"Résultats cohérents ?"}

        ML(["Modèle ML DMS"])
        S{"Proba en zone<br/>d'incertitude ?"}

        LOG[("Logs : agents appelés,<br/>sorties, erreurs, décisions")]
        MON["Monitoring<br/>agents + ML + latence"]
    end

    subgraph ETAB["Établissement (humains)"]
        HUM("Revue humaine / HITL<br/>qui · délai · trace")
        OUT[/"Score de risque de séjour prolongé<br/>→ équipe soignante"/]
    end

    DPI --> ORCH
    CR --> ORCH

    ORCH --> CTRL
    CTRL --> E

    E -->|oui| ADS
    E -->|oui| AEC
    E -.->|non| HUM

    ADS --> CONS
    AEC --> CONS

    CONS --> VAL
    VAL --> K

    K -->|oui| ML
    K -.->|non / conflit entre agents| HUM

    ML --> S
    S -->|non| OUT
    S -.->|oui| HUM

    HUM -.->|données corrigées / validées| CONS
    HUM -.->|validation du score| OUT

    ORCH --> LOG
    ADS --> LOG
    AEC --> LOG
    VAL --> LOG

    LOG -.-> MON
    ML -.-> MON

    classDef data fill:#e5e7eb,stroke:#6b7280,color:#111
    classDef code fill:#ffffff,stroke:#6b7280,color:#111
    classDef ml fill:#bfdbfe,stroke:#1d4ed8,color:#111
    classDef llm fill:#fed7aa,stroke:#c2410c,color:#111
    classDef dec fill:#fef08a,stroke:#a16207,color:#111
    classDef hum fill:#bbf7d0,stroke:#15803d,color:#111
    classDef out fill:#e9d5ff,stroke:#7e22ce,color:#111

    class DPI,CR,LOG data
    class ORCH,CTRL,CONS,VAL,MON code
    class ADS,AEC llm
    class ML ml
    class E,K,S dec
    class HUM hum
    class OUT out
```

**Principe** : 

Un agent orchestrateur se charge des entrées, et les transmet a un agent de controle qui établit la validité des entrants.
Si les données ne sont pas exploitables, une intervention humaine est requise.

Si les données le sont, alors un agent se charge d'analyser les données structurées (la base de données patient), pendant qu'un autre analyse les rapports écrits.

Les résultats sont transmis a un agent de validation, en charge alors de transmettre des données fiables au modèle ML qui va lui donner une prédiction pour le métier.

**Force** : 
- La modularité
- Gestion de workflows complexes
- Possibilité d'ajouter autant de controles que nécessaire
- Evolutif
- La traçabilité

**Faiblesse** :
- Complexité
- Latence supérieure
- Cout elvé
- Plus de points de panne potentiels
- Audit plus ardu

**Fallback** : 
- Le principal fallback est l'intervention humaine en cas de données insufisantes / incomplètes
- On peut également noter le conflits entre agents, ou le risque d'HITL
