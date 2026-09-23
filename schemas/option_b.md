# Option B — Hybride : le LLM extrait, le ML prédit

> **Légende (commune aux 3 schémas)** : `[( )]` données · `[ ]` traitement codé ·
> `([ ])` modèle ML · `{{ }}` LLM · `{ }` décision / seuil · `( )` humain ·
> `[/ /]` sortie · `-->` flux normal · `-.->` fallback

```mermaid
flowchart LR
    subgraph SIH["SI hospitalier (source)"]
        DPI[("DPI : données structurées")]
        CR[("Comptes-rendus<br/>(texte libre)")]
    end

    subgraph PF["Plateforme MediVox (équipe data)"]
        subgraph EXT["Extraction : 1 fois par document"]
            LLM{{"LLM extracteur<br/>schéma JSON imposé<br/>hébergé UE"}}
            V{"Validation<br/>schéma · plausibilité<br/>phrase source citée ?"}
            LOG[("Trace : version LLM,<br/>valeur, phrase source")]
        end
        FE["Pipeline de features<br/>structurées + extraites"]
        ML(["Modèle ML tabulaire"])
        S{"Proba en zone<br/>d'incertitude ?"}
        MON["CI/CD + monitoring<br/>+ échantillon de contrôle"]
    end

    subgraph ETAB["Établissement (humains)"]
        HUM1("Relecture extraction<br/>qui · délai · trace")
        HUM2("Revue du score<br/>qui · délai · trace")
        OUT[/"Score + explication<br/>→ équipe soignante"/]
    end

    DPI --> FE
    CR --> LLM --> V
    V -->|valide| FE
    V -.->|"incertain / invalide<br/>→ champ null"| FE
    V -.->|file de relecture| HUM1
    HUM1 -.->|valeur corrigée| FE
    V --> LOG
    FE --> ML --> S
    S -->|non| OUT
    S -.->|oui| HUM2
    HUM2 -.-> OUT
    ML -.-> MON
    LOG -.-> MON

    classDef data fill:#e5e7eb,stroke:#6b7280,color:#111
    classDef code fill:#ffffff,stroke:#6b7280,color:#111
    classDef ml fill:#bfdbfe,stroke:#1d4ed8,color:#111
    classDef llm fill:#fed7aa,stroke:#c2410c,color:#111
    classDef dec fill:#fef08a,stroke:#a16207,color:#111
    classDef hum fill:#bbf7d0,stroke:#15803d,color:#111
    classDef out fill:#e9d5ff,stroke:#7e22ce,color:#111
    class DPI,CR,LOG data
    class FE,MON code
    class ML ml
    class LLM llm
    class V,S dec
    class HUM1,HUM2 hum
    class OUT out
```

**Principe** : un LLM lit chaque compte-rendu et en extrait une liste de variables, 
sau format JSON imposé. Une fois validées, elles rejoignent les données structurées du dossier patient.

**Force** : exploite l'information du texte libre, absente du dossier, tout en
gardant une décision faite par un modèle ML. Chaque variable
extraite peut être vérifiée contre sa phrase source.

**Faiblesse** : ajoute une stack LLM (coût par document, hébergement de données
de santé, versioning du modèle) et un risque que le LLM hallucine.

**Fallback** : on peut en imaginer deux :
1. **Extraction** : si elle est incertaine ou invalide, on met null, on l'envoie en file de relecture et on
   ne l'injecte jamais telle quelle. On monitore régulièrement pour mesurer le taux d'erreur.
2. **Prédiction** : quand le modèle hésite il est marqué « incertain » et un humain tranche.