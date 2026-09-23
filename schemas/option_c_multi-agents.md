# Option C — Multi-agents

```mermaid
flowchart LR
    A[Données patient] --> B[Agent orchestrateur]
    C[Compte-rendu médical] --> B

    B --> D[Agent contrôle des entrées]

    D --> E{Données exploitables ?}

    E -->|Non| F[Revue humaine / HITL]

    E -->|Oui| G[Agent données structurées]
    E -->|Oui| H[Agent extraction clinique]

    G --> I[Variables consolidées]
    H --> I

    I --> J[Agent validation]

    J --> K{Résultat cohérent ?}

    K -->|Non| F
    K -->|Oui| L[Modèle ML DMS]

    L --> M{Confiance suffisante ?}

    M -->|Oui| N[Prédiction séjour prolongé]
    M -->|Non| F

    N --> O[Restitution métier]

    B --> P[Logs / traçabilité]
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
