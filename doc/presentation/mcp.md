# Slide — AILE via MCP

## Objectif
- Accompagner la préparation de vol libre en délivrant un assistant dialoguant météo, NOTAM et conseils tactiques.
- Montrer comment un assistant LLM peut structurer une aide à la décision en combinant prompts, données aéronautiques et expertise humaine.
- [Voir l’illustration associée](img/preview.png)

## Architecture MCP
- Un endpoint MCP déclenche la collecte de données officielles (météo, aérologie, navigation) et les incorpore au prompt.
- Le prompt structuré est envoyé au LLM qui produit une réponse, enrichie par les outils, puis le serveur renvoie cette synthèse au pilote.
- MCP orchestre l’ensemble, expose les outils disponibles et permet de documenter chaque appel.

## Décomposition du modèle
- Séparer les connaissances (sources officielles, bases SQLite, heures de décollage) des capacités de traitement (manipulation de prompts, raisonnement, synthèse).
- Cette grille aide les néophytes à comprendre ce qu’ils manipulent : des informations fixes d’un côté, et des capacités dynamiques de l’autre.

## Traçabilité et amélioration continue
- Documenter chaque prompt, chaque réponse, les calculs intermédiaires et les décisions prises par le modèle.
- Tenir des fichiers traçables permet aux informaticien·ne·s de retracer la chaîne (données → LLM → réponse) et d’itérer sur les modules LLM sans perdre en compréhension.
