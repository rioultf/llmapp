---
author:
- François Rioult
lang: fr
title: Application des LLM
subtitle: Agents MCP
---

# Objectif
- Accompagner la préparation de vol libre en délivrant un assistant dialoguant météo, NOTAM et conseils tactiques.
- Montrer comment un assistant LLM peut structurer une aide à la décision en combinant prompts, données aéronautiques et expertise humaine.
- <img src="../../fig/preview.png" />

# Architecture MCP

- Un endpoint MCP déclenche la collecte de données officielles (météo, aérologie, navigation) et les incorpore au prompt.
- Le prompt structuré est envoyé au LLM qui produit une réponse, enrichie par les outils, puis le serveur renvoie cette synthèse au pilote.
- MCP orchestre l’ensemble, expose les outils disponibles et permet de documenter chaque appel.

<img src="fig/agent.svg">

```mermaid
flowchart LR

  subgraph LLM[Structure de l'API pour LLM]
    req((query))

    subgraph Agent
      sys["sys prompt"]
      subgraph Provider
        OR[OR key]
        R{{LLM}}
      end
      sys --> R
    end


    req ==> R
    OR --> R
    R ==> rep((answer))
  end
```

```mermaid
flowchart LR
subgraph "Agent par orchestration d'outil"
    req((query))
    MCP
    code{{Local execution}}

    subgraph Agent["Orchestrator"]
        LLM1{{"LLM *with* tools"}}
        LLM2{{"LLM *no* tools"}}

    end
  
  LLM1 == "(tool, params)" ==> MCP
    MCP ==> code
    MCP -- "Tools" --> LLM1
    req ==> LLM1
    code ==> MCP
    MCP ==> LLM2
    req -.-> LLM2

  LLM2 ==> rep((answer))
end
```

# Décomposition du modèle

Il importe de bien séparer 

* les connaissances du modèle : informations fixes
* ses capacités de traitement (manipulation de prompts, raisonnement, synthèse) : capacités dynamiques

Question : *comment être sûr que le modèle fera appel à l'outil ? et pas à ses connaissances ?*

# Le protocole MCP

Un protocole extrèmement simple : STDIO, le serveur du pauvre.
Un script modulant `stdout` avec `stdin` suffit, et les échanges sont en JSON-RPC :

```bash
while read line; do
    echo "$line" | jq -Rc '{jsonrpc:"2.0", id:1, result:{received:.}}'
done
```

-- insérer ici la vidéo --

# Services MCP

```python
# server.py
from mcp.server.fastmcp import FastMCP

# Create an MCP server
mcp = FastMCP("Demo")

# Add an addition tool
@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b

@mcp.tool()
def level(sensor: int) -> int:
    """Returns the level at given sensor"""
    if sensor == 1:
        return 10.0
    return 24.0
```


