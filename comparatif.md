# Comparatif des 3 options — 4 dimensions (À COMPLÉTER)

| Dimension | A — ML classique                                                                                                    | B — Hybride LLM → ML                                                                                                                                           | C — Multi-agents                                                                                                                                                                                                                           |
|---|---------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Conformité** *(qualifiée)* | _niveau + 2 risques + maîtrise_                                                                                     | _…_                                                                                                                                                            | _…_                                                                                                                                                                                                                                        |
| **Performance** *(chiffrée)* | _F1 classe « prolongé », abstention, p95_                                                                           | _…_                                                                                                                                                            | _…_                                                                                                                                                                                                                                        |
| **Sobriété** *(chiffrée)* | 50€/mois, 0.0003€/inf. (ou €/doc)                                                                                   | 150 000 CRx 2000 tokens in + 150 out,a 0.25€/ M tokens in et 2€/M out, ~120€/mois + ~50€/mois de la solution A<br/>total: ~170/200€/mois soit 0.001€/document  | En supposant 3 appels LLM/dossier, environ 3x le cout de B donc 360€/mois de token + ML + orchestration.<br/> ~400/500€ /mois minimum, soit 0.002€/workflow, avant HITL                                                                    |
| **Évolutivité** *(qualifiée)* | OK pour scale le volume ou ajouter des variables structurées, mauvaise en cas de nouvelles sources non structurées. | Bonne, on peut intégrer des nouvelles sources structurées ou non structurées sans changer le prédicteur. <br/> Mauvaise en cas de changement de format des CR. | Très bonne, mais très couteuse. On peut ajouter a volonté des agents ou sources de façon modulaire.<br/>En revanche, cela augmentera de meme la complexité et la dépendance inter-agents, et peut rendre le tout plus complexe a observer. |

> **Évolutivité** = capacité à intégrer de **nouvelles sources de données**, à
> faire **évoluer le modèle** et à **augmenter le volume** sans refonte majeure.
> Même définition pour les 3 options — sinon vous comparez des choses différentes.

> **Chiffré vs qualifié** : seules **sobriété** et **performance** se chiffrent.
> Conformité et évolutivité se **qualifient** — niveau *faible / intermédiaire /
> fort* + justification + mesures de maîtrise. Une conformité « 7/10 » est une
> fausse précision.

> Chaque cellule : ordre de grandeur **chiffré** (ou niveau **qualifié**) +
> **risque principal**.

## Hypothèses (obligatoire)

Tout chiffre du tableau renvoie à une hypothèse listée ici. Ce sont des
**ordres de grandeur fondés sur des hypothèses explicites, pas des mesures de
production** : le but est de rendre les options comparables, pas de prédire le
coût réel.

| # | Hypothèse | Valeur retenue                                             | Source / date             |
|---|---|------------------------------------------------------------|---------------------------|
| H1 | Volume (dossiers/mois) | 5000 dossiers/jour x 30 jours = 150 000 dossier /mois      | Hypothèse de la ressource |
| H2 | Tokens moyens par compte-rendu (in / out) | 2000 tokens en entrée, 150 tokens en sortie                | Hypothèse de la ressource |
| H3 | Tarif du modèle retenu | GPT5-mini: 0.25€ / 1M tokens input, 2€ / 1M tokens outpout | Tarifs d'OpenAI           |

> ⚠️ **Performance de l'option B** : aucun gain annoncé sans le **protocole
> d'ablation** qui le prouverait (même modèle avec / sans les variables
> extraites, même jeu de test, même métrique) — cf. `ressources/02`.
