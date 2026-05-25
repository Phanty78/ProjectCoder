# Suivi du niveau — ProjectCoder

Échelle : qualitative (Acquis confirmé / Acquis fragile / Non acquis), + observations.

---

## Séance 2026-05-25 — Structure monorepo Bun

**Sujet** : audit et correction de la structure monorepo (workspaces Bun, lockfile, node_modules hoisting, tsconfig `extends`).

**Niveau de la séance** : bon raisonnement, application autonome partielle.

**Acquis confirmés cette séance**
- Comprend qu'un workspace vide casse `bun install` (vérifié en direct par l'erreur `Workspace not found "frontend"`).
- A diagnostiqué seul la cause racine du lockfile mal placé : `bun init` lancé dans `backend/` au lieu de la racine.
- A déduit, par raisonnement guidé, la distinction **source de vérité** (`package.json`, écrit à la main) vs **dérivés** (`bun.lock`, `node_modules/`, générés par `bun install`). Point fort de la séance.
- A compris pourquoi `backend/node_modules` se réinstalle (lié aux deps déclarées dans le `package.json` local).
- A identifié le mot-clé `extends` pour le tsconfig et formulé correctement le découpage base/spécifique.

**Acquis fragiles (à retester from-scratch)**
- Le pattern tsconfig `extends` : **concept compris mais non écrit par l'élève**. Il a demandé que je le fasse à sa place → bascule en « mode direct ». À retester en le faisant écrire lui-même.
- Plan de correction ordonné : ne l'a pas formulé avant d'agir, a exécuté directement (résultat correct, mais méthode = essai plutôt que plan).

**Patterns d'erreur / comportement à surveiller**
- **Tendance à l'offload** : dès qu'il s'agit d'écrire de la config (pas juste raisonner), demande à Claude de le faire à sa place, même après avoir trouvé le concept. À surveiller : reproduit-il ce réflexe sur d'autres tâches d'écriture ?
- A abandonné vite sur la question 3 (« je ne sais pas ») mais a rebondi correctement avec un seul indice de reformulation. Bonne réactivité une fois relancé.

**Axes à travailler (prochaines séances)**
- Faire écrire un tsconfig `extends` complet from-scratch, sans guidage.
- Exiger un **plan ordonné** avant exécution sur les tâches multi-étapes (résister à l'action immédiate).
- Travailler la persévérance sur le blocage avant de déléguer.

---

## Niveau global

**Profil** : raisonnement solide en mécanique d'outils (Bun, workspaces, résolution de dépendances). Bonne capacité de déduction guidée — trouve les bonnes réponses dès qu'on le relance par une question. 

**Limite principale** : passe du raisonnement à l'action en sautant la phase de plan, et délègue l'écriture de code/config plutôt que de la produire lui-même. Les concepts sont compris ; l'autonomie d'**exécution** reste à consolider.

**Une seule séance enregistrée à ce jour** — niveau global à confirmer sur les prochaines.
