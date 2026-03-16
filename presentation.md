# Séance 2h — Applications des LLM pour bac +3 professionnels

Cette session suppose que les participant·e·s maîtrisent la gestion de fichiers (textes ou données), savent travailler avec un terminal Linux sans recourir à la souris, conservent l’historique de leurs itérations via git et adoptent une démarche d’essais/tests itératifs pour fiabiliser leurs livrables. Dans ce contexte, la matinée vise à explorer les usages concrets des LLM en production, en privilégiant une stack minimaliste (terminal + API `chat_completions`).

## 1. Objectifs de la session
- Cartographier les principales applications métiers des LLM (agents conversationnels, supervision d’outils, RAG, distillation) et comprendre que le LLM est une brique interrogée via une API HTTP, ce qui constitue une introduction à la programmation par le texte.
- Montrer comment orchestrer une chaîne LLM + outil dans un scénario réaliste, avec vidéo/démonstration des flux d’appel.
- Préparer les participant·e·s à conduire une expérimentation itérative avec des prompts, des tests d’API et des critères de validation.

## 2. Déroulé (90 mn + 30 mn Q&A)

### Partie 1 — Usages & architecture (35 mn)
1. Présentation des applications que tu as développées : le pipeline Airflow IMAP → classification → SQLite (DAGs `imap_poll_classify_and_trigger`, `classify_emails`, `index_classification_to_sqlite`, prompt `prompts/prompt2.md` et modèle OpenRouter) et l’agent AILE (Assistant Intelligent pour le Libre-Élan) qui combine météo, NOTAM et conseils pour le vol libre.
2. Cartographie des composants : prompt, LLM, API `chat_completions`, opérateurs Airflow ou modules d’orchestration d’outils (MCP/AILE), échanges de données structurées.
3. Mise en perspective des contraintes opérationnelles : sécurité, latence, validation Airflow (données dicts, `dag_run.conf`), gestion des erreurs, induction d’un prompt strict.
4. Transition vers les démonstrations : quelles données on alimente, quel outil on appelle (ex. Airflow → SQLite, AILE → API météo + NOTAM).

### Partie 2 — Présentation d’un LLM (30 mn)
1. Reprise des fondamentaux de `00.Model.md` : architectures récurrentes, apprentissage par réseau de neurones et mécanisme d’attention qui permettent d’encoder l’entrée, produire un vecteur de contexte, puis décoder une réponse token par token.
2. Distinction claire entre la *connaissance* (datasets, embeddings, benchmarks, récompenses, système de notation des réponses) et la *manipulation du langage* (séquences de tokens, prompts, chat templates décrits dans `01.ModelBis.md`). On rappelle que la connaissance est statique, encodée dans les poids, tandis que la manipulation textuelle est dynamique et pilotée par nos instructions envoyées par API.
3. Illustration de l’API `chat_completions` comme point d’entrée : prompts structurés, rôles `system/user/assistant`, notion de température/top-k/top-p, et lecture concrète d’une requête HTTP qui incarne cette programmation par le texte.
4. Mise en perspective pédagogique : les élèves comprennent que leur interaction avec le LLM est une requête HTTP textuelle, écrite dans le terminal, et que l’outil manipule le texte comme un programme, avec des règles (attention, tokenisation, stop sequences) pour garantir la cohérence.

### Partie 3 — Prompt engineering (25 mn)
1. Application des règles de `prompt.md` : définir les objets mathématiques/techniques, séparer notion, équation et diagramme Mermaid, et terminer chaque section par un résumé fonctionnel clair. Montrer comment cela structure le prompt et assure la traçabilité de l’itération.
2. Atelier pratique : construire un promptFoo ou un chat template dans lequel les rôles (`system`, `user`, `assistant`) se succèdent en respectant les invites de la section. Intégrer la vérification de règles (première définition, diagramme) et l’usage d’un prompt modulable pour piloter la réponse.
3. Checklist de pilotage : promesses textuelles explicites, documentation des changements (édition/diff), invalidation rapide des prompts non conformes, et liens directs vers les ressources (scripts/TP, `prompt.md`, modèles).

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
