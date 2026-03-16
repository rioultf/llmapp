# Stack vidéo - Coding assisté par IA

---

## Environnement

- **OS** : Linux (avec un terminal moderne comme Alacritty ou GNOME Terminal)
- **Éditeur** : VS Code (idéalement avec Remote - Containers ou WSL)
- **Langages** : Python 3.11, JavaScript (Node.js 20), Shell
- **Gestionnaire de dépendances** : `pip`, `npm`, `uv`

---

## Outils IA

- **Modèle** : GPT (via API locale ou cloud)
- **Agent** : MCP Server Demo (`tp/script/mcp-server-demo`)
- **Intégrations utiles** :
  - Scripts `chatCompletions.sh` / `chatCompletions-LMS.sh`
  - Prompt manager `tp/script/promptFoo`
  - OpenWebUI (via `tp/script/owui`)

---

## Workflow

1. Initialisation du projet (`./tp/script/init.sh`).
2. Édition en split-view : code + terminal + fenêtre de chat IA.
3. Appel IA : `chatCompletions.sh` ou interface OpenWebUI.
4. Validation : exécution de tests spécifiques ou démonstration d’un agent (ex. MCP).
5. Captures d’écran de la collaboration homme/IA à chaque étape.

```
