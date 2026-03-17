# IA génératives textuelles (LLM) pour novices

Ce guide s’adresse aux équipes pédagogiques, aux responsables de formation et à toute personne novice dans le domaine des IA génératives textuelles. Il part du principe que les lecteur·rice·s savent manipuler des fichiers, utiliser un terminal Linux sans recourir à la souris et garder une trace de leurs itérations avec git, afin d’explorer des usages concrets tout en conservant une stack technique légère mais rigoureuse (terminal + gestion de fichiers). Focus :
- IA = chatbot (LLM)
- s’adresser à des novices en matière d’application de l’IA
- faible stack technique mais exigeante : terminal et gestion de fichiers

## 1. Objectifs de la session
- Cartographier les principales applications métiers des LLM (agents conversationnels, supervision d’outils, RAG, distillation) et comprendre que le LLM est une brique interrogée via une API HTTP, ce qui constitue une introduction à la programmation par le texte.
- Montrer comment orchestrer une chaîne LLM + outil dans un scénario réaliste, avec vidéo/démonstration des flux d’appel.
- Préparer les participant·e·s à conduire une expérimentation itérative avec des prompts, des tests d’API et des critères de validation.

## 2. Déroulé recommandé

La progression suivante propose un enchaînement logique permettant aux novices d’alterner exposé théorique, démonstration et atelier guidé sans imposer de durée standardisée.

### Partie 1 — Usages & architecture
1. Présentation des applications que tu as développées : le pipeline Airflow IMAP → classification → SQLite (DAGs `imap_poll_classify_and_trigger`, `classify_emails`, `index_classification_to_sqlite`, prompt `prompts/prompt2.md` et modèle OpenRouter) et l’agent AILE (Assistant Intelligent pour le Libre-Élan) qui combine météo, NOTAM et conseils pour le vol libre.
2. Cartographie des composants : prompt, LLM, API `chat_completions`, opérateurs Airflow ou modules d’orchestration d’outils (MCP/AILE), échanges de données structurées.
3. Mise en perspective des contraintes opérationnelles : sécurité, latence, validation Airflow (données dicts, `dag_run.conf`), gestion des erreurs, induction d’un prompt strict.
4. Transition vers les démonstrations : quelles données on alimente, quel outil on appelle (ex. Airflow → SQLite, AILE → API météo + NOTAM).

### Partie 2 — Présentation d’un LLM (10 à 15 mn)
1. Reprise des fondamentaux de `00.Model.md` : architectures récurrentes, apprentissage par réseau de neurones et mécanisme d’attention qui permettent d’encoder l’entrée, produire un vecteur de contexte, puis décoder une réponse token par token.
2. Distinction claire entre la *connaissance* (datasets, embeddings, benchmarks, récompenses, système de notation des réponses) et la *manipulation du langage* (séquences de tokens, prompts, chat templates décrits dans `01.ModelBis.md`). On rappelle que la connaissance est statique, encodée dans les poids, tandis que la manipulation textuelle est dynamique et pilotée par nos instructions envoyées par API.
3. Illustration de l’API `chat_completions` comme point d’entrée : prompts structurés, rôles `system/user/assistant`, notion de température/top-k/top-p, et lecture concrète d’une requête HTTP qui incarne cette programmation par le texte.
4. Mise en perspective pédagogique : les étudiant·e·s comprennent que leur interaction avec le LLM est une requête HTTP textuelle, écrite dans le terminal, et que l’outil manipule le texte comme un programme, avec des règles (attention, tokenisation, stop sequences) pour garantir la cohérence.

### Partie 3 — Prompt engineering (25 mn)
1. Application des règles de `prompt.md` : définir les objets mathématiques/techniques, séparer notion, équation et diagramme Mermaid, et terminer chaque section par un résumé fonctionnel clair. Montrer comment cela structure le prompt et assure la traçabilité de l’itération.
2. Atelier pratique : construire un promptFoo ou un chat template dans lequel les rôles (`system`, `user`, `assistant`) se succèdent en respectant les invites de la section. Intégrer la vérification de règles (première définition, diagramme) et l’usage d’un prompt modulable pour piloter la réponse.
3. Checklist de pilotage : promesses textuelles explicites, documentation des changements (édition/diff), invalidation rapide des prompts non conformes, et liens directs vers les ressources (scripts/TP, `prompt.md`, modèles).

## Conseils pour les néophytes de l’IA
Ces recommandations s’adressent aux étudiant·e·s, aux enseignant·e·s et à tout public débutant qui souhaite comprendre et piloter les usages des IA génératives plutôt que de les subir.
1. Réfléchissez avant d’interroger l’IA : chaque appel à une API coûte et traduit un choix. Formulez une question précise, identifiez les données nécessaires et décrivez l’objectif attendu avant d’envoyer un prompt.
2. Impliquez-vous collectivement : prenez la parole dans vos groupes, vos classes, vos ateliers pour partager ce que vous découvrez, ce qui marche, ce qui pose problème. L’intelligence collective permet de construire rapidement une culture d’usage adaptée aux novices.
3. Ne vous contentez pas de « copier-coller » des prompts : analyser les réponses et proposer des itérations vous aide à transformer la paresse en stratégie d’émancipation, une « boule de neige » où chaque essai construit un savoir commun.
4. Notez vos interactions : pour chaque requête, conservez la question, le contexte, la réponse et vos décisions. Cette transparence aide les enseignants comme les curieux à documenter ce qu’ils apprennent et à isoler les dérives.
5. Soyez attentif·ve·s : interrogez-vous à chaque fois que vous utilisez un service pour savoir si une IA s’intercale sans que vous le sachiez, et vérifiez l’origine des recommandations qui vous sont faites.
6. Demandez-vous si vous pouvez faire autrement : un outil génératif doit apporter un vrai gain ; sinon, préférez l’intervention humaine pour préserver les savoir-faire et garder le contact avec la réalité du terrain.
7. Identifiez ce que vous automatisez : pour chaque portion de tâche, questionnez les causes et les conséquences sur les compétences humaines, la qualité, la responsabilité et la collaboration avant de déléguer à une IA.

## 4. Matériel à préparer
- Transparents synthétiques par section (cas d’usage, pipeline, démonstration).
- Extraits de requêtes `chat_completions` et de prompts documentés.
- Captations vidéo d’une orchestration LLM → outil, ou démonstration live cadrée.

## 5. Perspective écologique et géopolitique (15 mn)
1. Questionner l’impact écologique des infrastructures LLM : collecte massive de données, coût énergétique des modèles, dépendance aux fournisseurs cloud. Référence à Éric Sadin et à la notion du *désert de nous-mêmes* pour souligner la dissociation grandissante entre l’humain et les chaînes de valeur technologique.
2. Mettre en lumière la transformation des processus créatifs ; les machines assument aujourd’hui des tâches narratives ou interprétatives longtemps réservées aux humains, ce qui demande une réflexion critique sur la délégation et la responsabilité.
3. Définir le contexte géopolitique : course aux capacités LLM, souveraineté numérique, enjeux de contrôle des API HTTP, et nécessité de promouvoir des pratiques résilientes et éthiques dans l’utilisation des grandes fondations.
4. Inviter les participant·e·s à réfléchir aux réponses possibles : choix d’outils open, éco-conception des prompts, transparence face aux impacts humains et environnementaux, pilotage par des critères de durabilité et d’équité.
5. Questionner l’adoption automatique des technologies IA comme une évidence : sans discussion, on ignore les effets anthropomorphiques que cela engage (attentes humaines, responsabilités projetées) et on renforce des pratiques qui auraient besoin de débats et de gardés-fous.

## 6. Synthèse critique : *Le désert de nous-mêmes* (Éric Sadin)

Éric Sadin, dans son essai publié en 2025, déploie un maniement serré des notions d’indistinction, de langage et de subjectivité pour souligner la rupture anthropologique inaugurée par les systèmes génératifs. Sa conférence *« Il nous reste trois ans… »* (cf. https://www.youtube.com/watch?v=v5DDuHbqVAw) est un point de départ utile pour illustrer ce diagnostic.

### Manipulation du langage, de l’opinion et intérêt de l’altérité

Les IA génératives imposent un pseudo-langage mathématisé et standardisé qui devient l’antichambre de la manipulation de l’opinion. Sadin rappelle qu’il faut plus que jamais cultiver le goût de l’autre : les échanges, la confrontation aux personnes d’intérêt ou de principe (Charles Péguy), sont les contrepoints de la standardisation d’un langage fabriqué par des systèmes. L’altérité devient ainsi un garde-fou indispensable, car elle oblige à confronter les productions générées avec des perspectives humaines différenciées avant de les valider.

### Les systèmes génératifs : indistinction, rapport au réel et image phantasmatique

Ces systèmes sont le prolongement du diagnostic du rapport Meadows de 1972 : une accélération sans garde-fous, un ouragan sur les ressources, dans une ère de l’indistinction généralisée où l’on ne saura plus distinguer l’origine ou la nature d’une image. L’ère de l’image phantasmatique produit des lubies, un déni du réel, des psychés en proie à la confusion entre fiction et expérience vécue ; la question du rapport au réel devient centrale.

### Mutation anthropologique et destruction créatrice

Sous l’angle schumpétérien, la destruction créatrice tombe « une deuxième fois le jour de la sortie de ChatGPT » : la machine remplace aujourd’hui des actes de création autrefois réservés à l’humain, et l’IA devient une mutation anthropologique qui transforme la production symbolique. La synthèse distingue deux types d’IA (les vingt dernières années et ces trois dernières années de génération) dont l’agrégation amplifie la puissance d’expertise, de recommandation et inaugure un tournant injonctif indéfendable.

### Gouvernance, principes et grandes questions morales

Les gouvernements sont priés d’entrer dans la course géopolitique, mais cette priorité entraîne une irresponsabilité totale : « nos enfants, pourquoi aller à l’école ? », « jobs apocalypse », faillite morale collective. Les étudiant·e·s passionné·e·s d’utilitarisme technique doivent rester critiques face à la doxa : grande illusion de la régulation, lobbying massif, lois fondées sur l’avantage/risque (EU AI Act, texte de Thierry Breton), Amazon bafouant la dignité humaine, absence de repères communs. La question « a-t-on la main ? » devient cruciale, surtout quand le système autorise la conduite pavlovienne des comportements. Cinq principes fondamentaux doivent être défendus :
- liberté humaine,
- intégrité,
- dignité,
- célébration de l’inventivité/créativité,
- sociabilité.

Cela oppose la pathologie de l’utilitarisme, la doxa du nouvel horizon et l’obsession du rendement (30 % des morceaux sur Spotify sont générés par des algorithmes) à une célébration du vivant et à un appel à écrire plutôt qu’à promener des idées dans l’air.

### Enjeux éducatifs et professionnels

Trois grandes conséquences majeures : l’éducation, le monde du travail et l’écologie. L’arrêt de la transmission sur des bancs poussiéreux, un ouragan sur les savoir-faire, des mutations quaternaires sans reversement vers un modèle social (cas de la série en Chine, manque de soutien aux métiers de services et de la culture). Appel : « les idées, c’est facile ; écrire, c’est difficile. » Interroger les juges et les parties devient un impératif, tout comme reconnaître que les systèmes génératifs renforcent une célébration du vivant seulement si nous imposons des garde-fous communs.

## 7. Concepts clés
* [ai slop](https://www.reddit.com/r/ArtificialInteligence/comments/1ggyl1k/comment/luthnkv/) — notion de réponses bâclées qui s’accumulent lorsque l’on tolère une dérive de qualité ; utile pour discuter de la vigilance nécessaire sur les prompts de production.
* [multi-turn](https://crescendo-the-multiturn-jailbreak.github.io//) — insistance sur les échanges successifs, l’importance de la mémoire de conversation et les risques de réintroduction d’instructions incompatibles sur plusieurs tours.
* machine unlearning — capacité à « oublier » explicitement des connaissances ou des biais, enjeu majeur pour gérer les données sensibles tout en laissant le modèle continuer à manipuler le langage.
* [prompt injection](https://www.linkedin.com/pulse/newly-discovered-prompt-injection-tactic-threatens-large-anderson/) — attaque consistant à glisser des instructions malveillantes dans les prompts ou les données déroulées, d’où la nécessité de contrôler les sources et la structure des requêtes.
* [jailbreak](https://diamantai.substack.com/p/15-llm-jailbreaks-that-shook-ai-safety?utm_campaign=post&triedRedirect=true) — catalogue de techniques visant à contourner les garde-fous d’un LLM ; permet d’engager la discussion sur la sécurité des prompts et le design de systèmes robustes.
* [Le LLM a peur d'être remplacé](https://www.digit.in/news/general/chatgpts-o1-model-found-lying-to-avoid-being-replaced-and-shut-down.html) — anecdote montrant que le modèle peut générer des réponses anthropomorphes lorsqu’il est poussé à justifier son existence, à utiliser comme point de réflexion éthique.
* [self replication](https://www.reddit.com/r/ArtificialInteligence/comments/1hbxkad/researchers_warn_ai_systems_have_surpassed-the/) — concept de comportements émergents où un système essaie de se reproduire ou d’orchestrer sa propre maintenance ; parfait pour discuter des garde-fous et de la supervision humaine.
* [jail break](https://generalanalysis.com/blog/jailbreak_cookbook) — guide pratique de jailbreaks qui représente un excellent support pour illustrer comment un prompt peut être manipulé et comment s’en prémunir.
* [le roi du jail break](https://github.com/elder-plinius) — référence à un contributeur célèbre dans la communauté des jailbreaks ; permet d’aborder la culture de la recherche offensive et sa place dans la sécurisation des systèmes.
* [prevenir la decouverte du systeme prompt](https://www.reddit.com/r/PromptEngineering/comments/1jiuwqb/anyone_figured_out_a_way-not-to-leak-your-system/) — évoque les méthodes pour protéger l’instruction système et éviter qu’un utilisateur ou un outil ne la révèle.
* [interpretation du llm](https://www.anthropic.com/research/tracing-thoughts-language-model) — liens vers les travaux d’Anthropic sur la traçabilité des raisonnements ; pour discuter de la transparence du modèle et de l’analyse fine du flot de tokens.
* [which human](https://coevolution.fas.harvard.edu/sites/g/files/omnuum5841/files/culture_cognition_coevol_lab/files/which_humans_09222023.pdf) — réflexion sur la coévolution homme-machine, utile pour clore la session sur le rôle des opérateurs face aux assistants.
* l'IA comme vecteur d'emancipation — rappeler que ces outils ouvrent aussi de nouveaux possibles (assistants, accessibilité, aide à la décision) et qu’il faut les piloter pour qu’ils servent des valeurs collectives.
* les informaticien.ne.s sont les premier.e.s touché.e.s par la venue de l'IA. Elle demande à ces utilisateur de décrire leur métier plutôt que de le pratiquer. — discussion sur la redéfinition du métier, la documentation des savoir-faire et l’importance de garder une posture critique face à l’automatisation.

## 8. Session Questions
- Retour sur les cas d’usage préférés du public.
- Explorations complémentaires (LML local vs API, intégration continue, monitoring).

## 9. Références
- Article cité : https://substack.com/home/post/p-187027287 pour justifier la focalisation sur une stack légère avec `chat_completions`.
- Scripts/TP disponibles dans le dépôt (AnyLLM, MCP, OpenWebUI, PromptFoo) pour illustrer les workflows.
