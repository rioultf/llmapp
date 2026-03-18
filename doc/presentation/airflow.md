# Slide — Pipeline d’indexation de mails

1. Objectif : automatiser la collecte d’emails, leur classification puis leur indexation dans une base SQLite pour pouvoir interroger les conversations historiques ou détecter des priorités.
2. Architecture :
   - Airflow orchestre les DAGs `imap_poll_classify_and_trigger`, `classify_emails` et `index_classification_to_sqlite`.
   - Chaque DAG interroge un LLM via des prompts (par exemple `prompts/prompt2.md`) pour obtenir des classifications et des résumés.
   - Les résultats sont stockés dans SQLite, alimentant tableaux de bord, recherches en texte intégral ou agents conversationnels.
3. Pourquoi c’est pédagogique :
   - Les novices manipulent prompts, API HTTP `chat_completions` et orchestration par terminal.
   - La traçabilité (prompts, réponses, données) structure la documentation et facilite l’apprentissage complet : extraction, traitement, stockage.
4. À retenir :
   - Chaque bloc (IMAP → classification → stockage) permet d’aborder gouvernance des données, qualité et responsabilité humaine.
   - L’agent AILE combine météo, NOTAM et conseils de vol tout en gardant le contrôle sur les sources et les prompts.
