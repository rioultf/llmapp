# Séance 2h — Applications des LLM pour bac +3 professionnels

Cette session suppose que les participant·e·s maîtrisent la gestion de fichiers (textes ou données), savent travailler avec un terminal Linux sans recourir à la souris, conservent l’historique de leurs itérations via git et adoptent une démarche d’essais/tests itératifs pour fiabiliser leurs livrables. Dans ce contexte, la matinée vise à explorer les usages concrets des LLM en production, en privilégiant une stack minimaliste (terminal + API `chat_completions`).

## 1. Objectifs de la session
- Cartographier les principales applications métiers des LLM (agents conversationnels, supervision d’outils, RAG, distillation).
- Montrer comment orchestrer une chaîne LLM + outil dans un scénario réaliste, avec vidéo/démonstration des flux d’appel.
- Préparer les participant·e·s à conduire une expérimentation itérative avec des prompts, des tests d’API et des critères de validation.

## 2. Déroulé (90 mn + 30 mn Q&A)

### Partie 1 — Usages & architecture (35 mn)
1. Présentation rapide de cas d’usage réels : service client, agent de supervision, déploiement d’un RAG.
2. Cartographie des composants : prompt, LLM, API `chat_completions`, outils tiers, orchestration.
3. Mise en perspective des contraintes (sécurité, coût, latence) et des indicateurs de succès.
4. Transition vers les démonstrations : quelles données on alimente, quel outil on appelle.

### Partie 2 — Démonstration / orchestration (30 mn)
1. Présentation du scénario choisi (ex. agent orchestration d’un outil ou gestion de pipeline RAG).
2. Lecture guidée d’un promptFoo ou d’une requête `chat_completions` (explication des rôles système/utilisateur).
3. Vidéo ou démonstration live : enchaînement de requêtes API, appels d’outils, publications de résultats.
4. Discussion sur la robustesse : gestion des erreurs, limites du modèle, boucles de rétroaction.

### Partie 3 — Atelier de synthèse & bonnes pratiques (25 mn)
1. Séquence interactive : comment formuler un prompt modulable, comment documenter les itérations.
2. Présentation d’une checklist pour piloter un projet LLM (tests, métriques, sécurité, maintenabilité).
3. Mise en évidence des ressources disponibles (script terminal, modèles, processus d’orchestration).

### Session Questions (30 mn)
- Retour sur les cas d’usage préférés du public.
- Explorations complémentaires (LML local vs API, intégration continue, monitoring).

## 3. Ressources mentionnées
- Article cité : https://substack.com/home/post/p-187027287 pour justifier la focalisation sur une stack légère avec `chat_completions`.
- Scripts/TP disponibles dans le dépôt (AnyLLM, MCP, OpenWebUI, PromptFoo) pour illustrer les workflows.

## 4. Matériel à préparer
- Transparents synthétiques par section (cas d’usage, pipeline, démonstration).
- Extraits de requêtes `chat_completions` et de prompts documentés.
- Captations vidéo d’une orchestration LLM → outil, ou démonstration live cadrée.
