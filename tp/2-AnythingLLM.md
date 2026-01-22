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

# Installation et exécution `AnythingLLM`

Il faut télécharger soi-même `AnythingLLM` et lancer l'appimage. **Ne pas utiliser la version installée sur la VDI.**

Pour une [installation `AnythingLLM`](https://docs.anythingllm.com/installation-desktop/linux#install-using-the-installer-script) :

```
curl -fsSL https://cdn.anythingllm.com/latest/installer.sh -o installer.sh
 
# Make the script executable
chmod +x installer.sh
 
# Run the script
./installer.sh
```

## Commentaires

2.3G de téléchargement : pourquoi ? `AnythingLLM` utilise ses propres modèles et logiciels pour :

* base de données vectorielle
* préférence d'intégration - *embedding*. qualité vs latence vs couverture linguistique - 23 MB
* *chunking*
* whisper - 60MB, native TTS
* transcription OCR ?

# Paramétrage

## Fournisseur d'IA

* Generic OpenAI : permet d'interroger toute API `chat_completions` dont on connaît l'URL. Utile pour inférer sur un modèle local, servi par le *registre* adéquat.
* OpenRouter : copier la clé d'API

Les autres paramètres concernent la fourniture d'autres IA&nbsp;: base de données documentaires, embedding, chunking, voix, modèle de transcription de parole.

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

# Notes sur le système de fichier

```bash
export STORAGE_LOCATION=$HOME/anythingllm && \
mkdir -p $STORAGE_LOCATION && \
touch "$STORAGE_LOCATION/.env"
```

Often, you may want to write or even read files from the host machine - since the MCP Server runs within the context of the container - you must use this path:

/app/server/storage/...

This path will then be using the STORAGE_LOCATION directory that you defined when you started the AnythingLLM Docker container. From here you can then write and read files to the host machine.

### Notes

Not all LLM Models works well as Agents, you may need to use higher quantization models for better responses. Example: Llama 3 8B 8Bit Quantization gives better responses as an Agent

# Poursuite du travail

[Intégration MCP pour `AnythingLLM`](5.MCP.md)

