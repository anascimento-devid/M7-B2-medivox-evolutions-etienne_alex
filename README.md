# M7-B2 — Comparer 3 évolutions architecturales (MediVox)

> **Repo template.** Binôme par affinité technique. « Use this template » →
> `M7-B2-medivox-evolutions-<binome>`. **Pas de code** — conception et arbitrage.
> Restitution orale **en ouverture de M8** (15 min, sur vos schémas — pas de slides).

---

## 🧭 Votre brief en un coup d'œil

| Support | Rôle |
|---|---|
| **Simplonline** | Le contrat : contexte, livrables, critères |
| **Ce README** | Le pilotage : quoi produire, avec quel mini-cours |
| [`ressources/`](./ressources/) | 7 mini-cours (index dans [`ressources/README.md`](./ressources/README.md)) |
| **Discord `fil-M7-B2`** | Questions communes |

### L'async binôme (jeudi + vendredi matin, 5 h 30)

| Étape | À produire | Fichier | Appui |
|---|---|---|---|
| 1 | 3 schémas Mermaid (convention cohérente) | `schemas/option_{a,b,c}_TEMPLATE.md` | `01`, `02`, `03` |
| 2 | Comparatif 3×4 **chiffré** | `comparatif_TEMPLATE.md` | `04` |
| 3 | Fallback strategies par option | (dans la note) | `05` |
| 4 | Note 3 pages maximum + **recommandation UNE** | `note_comparaison_TEMPLATE.md` | `01`, `05` |
| 5 | **Répétition duo chronométrée 15 min** (oral sur les schémas, pas de slides) | — | — |

Renommez les `*_TEMPLATE` en versions finales.

> ⚠️ La restitution a lieu **en ouverture de M8**, pas cette semaine :
> figez schémas + note **vendredi** — dans 10 jours vous ne saurez plus
> pourquoi vous aviez écarté l'option C. Au freeze, écrivez en tête de note
> la **décision en une phrase** (option + raison chiffrée +
> condition de changement d'avis) : c'est elle que vous relirez le matin
> du 6. Le journal de bord est votre assurance-mémoire.

> 🧩 **Option B = hybride** : un LLM **extrait** des variables des
> comptes-rendus (validées, relues si incertaines), le **modèle ML prédit**.
> Un assistant RAG qui *répond* aux équipes est un **autre produit** —
> mentionnez-le à part s'il répond à un besoin, ne le comparez pas au prédicteur.

### ✅ Checklist livrables (avant vendredi 17h)

- [ ] 3 schémas **comparables** (mêmes formes/couleurs)
- [ ] Comparatif 3×4 **chiffré** (ordres de grandeur honnêtes, pas « cher »)
- [ ] **Fallback strategies** explicitées par option (seuil / abstention / HITL)
- [ ] **UNE** recommandation tranchée, argumentée chiffrée
- [ ] **Garde-fou sobriété explicite** : justifiez le choix (ou non) d'une
      approche LLM. *« 5 agents pour prédire un séjour prolongé »* = signal négatif
- [ ] **Décision en une phrase** en tête de note
- [ ] Répétition duo chronométrée faite (10 min + 5 min Q&A)
- [ ] **Journal de bord** tenu

## ⭐ Extension (non notée, si socle bouclé) — l'autre produit, mesuré

L'équipe d'Hélène parle de « RAG » : prenez-la au mot sur le seul besoin
où un RAG a du sens — **un assistant documentaire** qui répond aux équipes
à partir des documents internes. Montez un mini-RAG mesuré sur le corpus
MediVox (distribué sur Discord, avec son jeu de 12 questions
d'évaluation) — stack du bonus M3-B1 : `sentence-transformers` + ChromaDB,
**sans LangChain**, stop au retrieval. Mesurez **hit@3**, fixez un **seuil
d'abstention** et testez-le sur les 2 questions pièges. Puis ajoutez à
votre note un encadré « **besoin distinct** » : quel besoin ce produit
sert-il, qu'a montré la mesure — et pourquoi **aucune** ligne de votre
comparatif du prédicteur ne bouge. Savoir dire « bon outil, autre
problème » vaut plus cher qu'un arbitrage de lecture.

## 📚 Ressources

Voir [`./ressources/`](./ressources/) — 6 mini-cours (dont fine-tuning ⭐) + `liens_officiels.md`.
