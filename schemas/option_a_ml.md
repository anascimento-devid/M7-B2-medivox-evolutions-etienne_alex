# Option A — ML classique modernisé

> **Légende (commune aux 3 schémas)** : `[( )]` données · `[ ]` traitement codé ·
> `([ ])` modèle ML · `{{ }}` LLM · `{ }` décision / seuil · `( )` humain ·
> `[/ /]` sortie · `-->` flux normal · `-.->` fallback

```mermaid
flowchart LR
    subgraph SIH["SI hospitalier (source)"]
        DPI[("DPI : données structurées")]
        CR[("Comptes-rendus<br/>(texte libre)<br/>non exploités")]
    end

    subgraph PF["Plateforme MediVox (équipe data)"]
        FE["Pipeline de features<br/>structurées"]
        ML(["Modèle ML tabulaire"])
        S{"Proba en zone<br/>d'incertitude ?"}
        LOG[("Trace : version modèle,<br/>entrées, score")]
        MON["CI/CD + monitoring"]
    end

    subgraph ETAB["Établissement (humains)"]
        HUM2("Revue du score<br/>qui · délai · trace")
        OUT[/"Score + explication<br/>→ équipe soignante"/]
    end

    DPI --> FE
    FE --> ML --> S
    S -->|non| OUT
    S -.->|oui| HUM2
    HUM2 -.-> OUT
    ML --> LOG
    ML -.-> MON
    LOG -.-> MON

    classDef data fill:#e5e7eb,stroke:#6b7280,color:#111
    classDef code fill:#ffffff,stroke:#6b7280,color:#111
    classDef ml fill:#bfdbfe,stroke:#1d4ed8,color:#111
    classDef llm fill:#fed7aa,stroke:#c2410c,color:#111
    classDef dec fill:#fef08a,stroke:#a16207,color:#111
    classDef hum fill:#bbf7d0,stroke:#15803d,color:#111
    classDef out fill:#e9d5ff,stroke:#7e22ce,color:#111
    classDef unused fill:#f9fafb,stroke:#9ca3af,stroke-dasharray:5 5,color:#9ca3af
    class DPI,LOG data
    class FE,MON code
    class ML ml
    class S dec
    class HUM2 hum
    class OUT out
    class CR unused
```

**Principe** : on garde le modèle ML actuel, entraîné sur les données
structurées du dossier patient, et on l'industrialise : réentraînement et
déploiement automatisés (CI/CD), suivi de ses performances en production
(monitoring), traçabilité de chaque score.

**Force** : simple, sobre (aucun coût par document, pas de GPU), explicable,
et les données de santé ne quittent pas la plateforme. C'est la **référence** à
laquelle B et C doivent prouver un gain.

**Faiblesse** : ignore tout ce qui n'est écrit que dans les comptes-rendus
(autonomie, entourage, complications notées). Les patients dont le profil
dépend de ces infos restent dans la zone d'incertitude.

**Fallback** : quand le modèle hésite, le score est marqué « incertain » et un
humain tranche (même procédure que l'option B : qui, délai, pouvoir, trace).
