---
author:
- François Rioult
lang: fr
title: Application des LLM
subtitle: Environnement de développement
---

[AnythingLLM](https://docs.anythingllm.com/) :

* permet d'exposer l'API d'un agent, valorisant un provider (instance/admin/pref/enable network discovery)
* gestion de clé d'accès API
* plugin navigateur
* peut définir agents en appelant des serveurs MCP.

On ne l'utilisera pas pour interroger de façon critique un modèle personnalisé, l'interface n'est pas faite pour cela. Pour cet usage, on préférera `LMStudio` ou `OpenWebUi`.

# Installation

<https://docs.anythingllm.com/installation-desktop/linux#install-using-the-installer-script>

2.3G

# Paramétrage

## Fournisseur d'IA

* Generic OpenAI : permet d'interroger toute API `chat_completions` dont on connaît l'URL. Utile pour inférer sur un modèle local, servi par le *registre* adéquat.
* OpenRouter : copier la clé d'API

Les autre paramètres concernent la fourniture d'autres IA : base de données documentaires, embedding, chunking, voix, modèle de transcription de parole.

## Admin

* activer `Enable network discovery` pour que l'API mise en place par `AnythingLLM` soit visible sur le réseau

* définition du *system prompt*, langage de template

## Compétences de l'agent

* Agent Skills : RAG, résumé, scrape, generate, search, SQL connector
* Custom Skills : 
* Agent Flows
* MCP Servers

## Outils

* générer des clés d'API. C'est ici qu'on peut tester l'API avec une interface `swagger`
* variables du système prompt

# Espace de travail

C'est assez pauvre et peu paramétrable.

# TP - Mise en place d'un agent MCP

## Correction du bug à l'initialisation

1. lancer `anythingllm`
1. aller directement dans `admin/préférence de l'agent` -> n'arrive pas à démarrer les services MCP
1. forcer à quitter
1. éditer le fichier ~/.config/anythingllm-desktop/storage/plugins/anythingllm_mcp_servers.json

## Environnement python

* installer `uv`, gestionnaire d'environnement `python`.

```bash    
curl -LsSf https://astral.sh/uv/install.sh | sh

uv init mcp-server-demo
cd mcp-server-demo/
uv add "mcp[cli]"
source .venv/bin/activate
```
* y mettre le fichier de [description des services MCP](script/mcp-server-demo/mcp-server-demo.py)

```
mcp dev mcp-server-demo.py
```

After that I can connect to http://127.0.0.1:6274 and test the server.

The server has to be launched in the correct environment.

This is also true when using AnythingLLM is you want him to correctly launch your servers.

export STORAGE_LOCATION=$HOME/anythingllm && \
mkdir -p $STORAGE_LOCATION && \
touch "$STORAGE_LOCATION/.env"






### Notes

Not all LLM Models works well as Agents, you may need to use higher quantization models for better responses. Example: Llama 3 8B 8Bit Quantization gives better responses as an Agent

