# Stack vidéo - Coding assisté par IA

---

## Objectifs pédagogiques (cf. `readme.md`)

- Illustrer les notions clés d’un cours sur les LLM : compréhension du modèle, prompt engineering, agents et orchestration.
- Montrer la gestion itérative des fichiers, la traçabilité via `git` et la posture Linux recommandée.
- Valoriser les TP existants (`tp/0-VDI` à `tp/6.OpenWebUi`) qui jalonnent la montée en compétence.

---

## Environnement technique

- **OS** : Linux, avec terminal orienté productivité (GNOME Terminal, Alacritty, tmux).
- **Éditeur** : VS Code (Remote - Containers ou équivalent) pour juxtaposer édition + résultats.
- **Langages** : Python 3.11, Shell et JavaScript/Node.js (cf. scripts `tp/script/*`).
- **Outils de gestion** : `git`, `pip`, `npm`, `uv`, et scripts personnalisés (`init.sh`, `chatCompletions*.sh`).

---

## Outils IA et agentiels

- **Modèle** : GPT via API locale ou cloud, avec prompts systématisés (PromptFoo, `prompt.json`).
- **Agent** : MCP Server Demo (`tp/script/mcp-server-demo`) illustrant l’orchestration de fonctions/ressources.
- **Intégrations complémentaires** : OpenWebUI (`tp/script/owui`), AnythingLLM, promptFoo (TP 4), scripts de démonstration.

---

## Workflow projet

1. Démarrer l’environnement Linux (TP 0) puis lancer `./tp/script/init.sh`.
2. Naviguer entre VS Code, terminal et interface IA (split-view, capture continue).
3. Interroger l’IA (PromptFoo / `chatCompletions.sh`) puis appliquer les suggestions dans le code.
4. Exécuter les tests ou l’agent (MCP, OpenWebUI, etc.) pour valider les retours.
5. Capturer chaque étape pour les slides et relier le tout aux chapitres du cours (`CM 0` à `CM 4`).

```
