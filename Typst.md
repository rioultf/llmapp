---
author: François Rioult
title: `typst` - programmation de document
---

# Documentation

La documentation de `typst` est un manuel de référence. Il faut savoir ce qu'on cherche. Pour le reste, c'est famélique. Et `chatgpt` n'est pas fort en `typst`, décevant même. 

* <https://github.com/typst/typst>
* [Manuel de la CLI](https://docs.rs/crate/typst-cli/latest)

* exports : PDF, HTML, PNG, Markdown

## Packages

`typst` propose une vaste gamme de *template*, dont le code peut être examiné sur <https://github.com/typst/packages/blob/main/packages/preview/>

https://github.com/typst/packages/blob/main/packages/preview/basic-academic-letter/0.1.0/lib.typ)

# Ingénierie documentaire avec `Typst`

L’ingénierie documentaire se situe à l’intersection de la programmation, des systèmes d’information et des langages de description. Elle considère les artefacts manipulés par l’informatique moderne comme des *documents* au sens large.

Dans ce cadre :

* une base de données est un document structuré,
* une page web est un document composite,
* une configuration, un schéma, un log ou une API sont des documents formels.

L’unité fondamentale n’est plus le fichier ou la page, mais la *structure informationnelle* transformable.

## Documents comme objets de transformation

L’ingénierie documentaire peut être formalisée comme une suite de transformations :

* ingestion d’un document source,
* enrichissement, filtrage ou restructuration,
* projection vers un ou plusieurs formats cibles.

Chaque transformation est gouvernée par :

* une logique explicite,
* un contenu manipulé,
* une composition adaptée au contexte de sortie.

Ce modèle s’applique indifféremment à :

* la génération de documents pédagogiques,
* la production de pages web,
* la sérialisation de données,
* l’orchestration de chaînes de traitement.

### Lien avec les paradigmes informatiques actuels

Ce positionnement rejoint plusieurs tendances majeures :

* programmation déclarative,
* séparation données / logique / présentation,
* pipelines de transformation,
* reproductibilité et traçabilité.

L’ingénierie documentaire fournit ainsi un cadre conceptuel unificateur pour comprendre des systèmes hétérogènes comme des cas particuliers de transformation de documents.

### Synthèse conceptuelle

* tout artefact informationnel est un document,
* toute opération logicielle est une transformation de document,
* la valeur ajoutée réside dans la maîtrise explicite de ces transformations.

Ce point de vue permet d’aborder l’informatique non comme une juxtaposition de technologies, mais comme une discipline de structuration et de *circulation du sens*.

## Logique

La *logique* désigne l’ensemble des règles, calculs et décisions qui déterminent **quoi produire** et **dans quelles conditions**.
Elle inclut :

* les variables et paramètres,
* les tests conditionnels,
* les itérations,
* les fonctions et abstractions.

La logique ne décrit pas directement la forme du document ; elle gouverne la génération du contenu et sa structuration. Elle rend le document paramétrable, adaptable et reproductible.

### Contenu

Le *contenu* correspond à l’information sémantique portée par le document, indépendamment de sa présentation.
Il inclut :

* le texte,
* les formules,
* les données,
* les éléments conceptuels (définitions, résultats, arguments).

Le contenu est ce qui a vocation à être compris, cité ou réutilisé. Il doit rester stable même si la mise en forme ou le support évoluent.

### Composition

La *composition* est le processus par lequel le contenu, piloté par la logique, est **transformé en un document final**.
Elle recouvre :

* l’organisation spatiale,
* la typographie,
* la hiérarchie visuelle,
* la pagination et le rendu.

La composition répond à la question *comment le contenu est présenté*, sans en modifier le sens.

### Synthèse

* la logique décide,
* le contenu signifie,
* la composition matérialise.

L’ingénierie documentaire consiste à maintenir une séparation claire entre ces trois dimensions tout en assurant leur articulation cohérente.

# Architecture

Typst est un système moderne de composition de documents qui combine la puissance d’un langage de programmation à la simplicité d’un langage de balisage. 

Typst n’est pas seulement une succession de commandes de mise en page : c’est un langage intégré, pensé dès le départ pour être intuitif, puissant et lisible.

Langage de script intégré : fonctions, morceaux de mise en page, calculer du contenu de manière dynamique, ce qui en fait à la fois un langage de mise en page et un langage de programmation pour documents.

* écrire des documents structurés,

* automatiser du contenu avec du code,

* maîtriser la mise en page,

* et créer des templates réutilisables.

Typst s’impose comme une alternative moderne à des systèmes classiques comme LaTeX : syntaxe plus claire et plus courte, capacité à intégrer de la logique programmée dans le document, meilleures erreurs et expérience de développement, aucun besoin d’importer des paquets externes pour les fonctionnalités de base

# Mise en oeuvre

* Installation `snap`

```bash
ref=... typst compile reco.typ pdf/$ref.pdf --input data=data/$ref.json
```
* éditeur dynamique en ligne : à recommander car il est très interactif : suggestions (utiliser `Ctrl-Space`), préremplissage, signalisation d'erreurs.
* plugin codium `Tinymist Typst` : en haut du source, cliquer sur `Preview`

---

`typst` peut assembler des fichiers, images ou autre `typst`. Le langage de template est un langage au sens classique du terme, avec des variables, des tests et des boucles. Il mélange itération, fonctionnel et données/fichiers.

En programmant la mise en page précise, `typst` est un langage de composition de document plus puissant que LaTeX, qui est majoritairement fondé sur les classes de documents.

## Modes 

* code : préfixé par `#`
* math : entouré de `$`
* markup : `[...]`

## Markup mode

| Concept          | Syntaxe                  | Nom          |
| ---------------- | ------------------------ | ------------ |
| Paragraph break  | Ligne vide               | `parbreak`   |
| Strong emphasis  | `*strong*`               | `strong`     |
| Emphasis         | `_emphasis_`             | `emph`       |
| Raw text         | `` `print(1)` ``         | `raw`        |
| Link             | `https://typst.app/`     | `link`       |
| Label            | `<intro>`                | `label`      |
| Reference        | `@intro`                 | `ref`        |
| Heading          | `= Heading`              | `heading`    |
| Bullet list      | `- item`                 | `list`       |
| Numbered list    | `+ item`                 | `enum`       |
| Term list        | `/ Term: description`    | `terms`      |
| Math             | `$x^2$`                  | `math`       |
| Line break       | `\`                      | `linebreak`  |
| Smart quote      | `'single'` ou `"double"` | `smartquote` |
| Symbol shorthand | `~`, `---`               | `symbols`    |
| Code expression  | `#rect(width: 1cm)`      | `scripting`  |
| Character escape | `\#`                     | `escape`     |
| Comment          | `/* block */`, `// line` | `comment`    |

## Math mode

| Name                   | Example               | See         |
| ---------------------- | --------------------- | ----------- |
| Inline math            | `$x^2$`               | `math`      |
| Block-level math       | `$ x^2 $`             | `math`      |
| Bottom attachment      | `$x_1$`               | `attach`    |
| Top attachment         | `$x^2$`               | `attach`    |
| Fraction               | `$1 + (a+b)/5$`       | `frac`      |
| Line break             | `$x \ y$`             | `linebreak` |
| Alignment point        | `$x &= 2 \ &= 3$`     | `math`      |
| Variable access        | `$#x$`, `$pi$`        | `math`      |
| Field access           | `$arrow.r.long$`      | `scripting` |
| Implied multiplication | `$x y$`               | `math`      |
| Symbol shorthand       | `$->$`, `$!=$`        | `symbols`   |
| Text/string in math    | `$a "is natural"$`    | `math`      |
| Math function call     | `$floor(x)$`          | `math`      |
| Code expression        | `$#rect(width: 1cm)$` | `scripting` |
| Character escape       | `$x\^2$`              | `escape`    |
| Comment                | `$/* comment */$`     | `comment`   |

## Code mode

| Name                     | Example                       | See          |
| ------------------------ | ----------------------------- | ------------ |
| None                     | `none`                        | `none`       |
| Auto                     | `auto`                        | `auto`       |
| Boolean                  | `false`, `true`               | `bool`       |
| Integer                  | `10`, `0xff`                  | `int`        |
| Floating-point number    | `3.14`, `1e5`                 | `float`      |
| Length                   | `2pt`, `3mm`, `1em`, `..`     | `length`     |
| Angle                    | `90deg`, `1rad`               | `angle`      |
| Fraction                 | `2fr`                         | `fraction`   |
| Ratio                    | `50%`                         | `ratio`      |
| String                   | `"hello"`                     | `str`        |
| Label                    | `<intro>`                     | `label`      |
| Math                     | `$x^2$`                       | `math`       |
| Raw text                 | `` `print(1)` ``              | `raw`        |
| Variable access          | `x`                           | `scripting`  |
| Code block               | `{ let x = 1; x + 2 }`        | `scripting`  |
| Content block            | `[*Hello*]`                   | `scripting`  |
| Parenthesized expression | `(1 + 2)`                     | `scripting`  |
| Array                    | `(1, 2, 3)`                   | `array`      |
| Dictionary               | `(a: "hi", b: 2)`             | `dictionary` |
| Unary operator           | `-x`                          | `scripting`  |
| Binary operator          | `x + y`                       | `scripting`  |
| Assignment               | `x = 1`                       | `scripting`  |
| Field access             | `x.y`                         | `scripting`  |
| Method call              | `x.flatten()`                 | `scripting`  |
| Function call            | `min(x, y)`                   | `function`   |
| Argument spreading       | `min(..nums)`                 | `arguments`  |
| Unnamed function         | `(x, y) => x + y`             | `function`   |
| Let binding              | `let x = 1`                   | `scripting`  |
| Named function           | `let f(x) = 2 * x`            | `function`   |
| Set rule                 | `set text(14pt)`              | `styling`    |
| Set-if rule              | `set text(..) if ..`          | `styling`    |
| Show-set rule            | `show heading: set block(..)` | `styling`    |
| Show rule with function  | `show raw: it => {..}`        | `styling`    |
| Show-everything rule     | `show: template`              | `styling`    |
| Context expression       | `context text.lang`           | `context`    |
| Conditional              | `if x == 1 {..} else {..}`    | `scripting`  |
| For loop                 | `for x in (1, 2, 3) {..}`     | `scripting`  |
| While loop               | `while x < 10 {..}`           | `scripting`  |
| Loop control flow        | `break`, `continue`           | `scripting`  |
| Return from function     | `return x`                    | `function`   |
| Include module           | `include "bar.typ"`           | `scripting`  |
| Import module            | `import "bar.typ"`            | `scripting`  |
| Import items from module | `import "bar.typ": a, b, c`   | `scripting`  |
| Comment                  | `/* block */`, `// line`      | `comment`    |


## Routine

On utilise l'interface web et on visualise instantanément le rendu.

* le `\#` introduit un appel du langage, tout le reste est du texte
* `#{...}` est un *bloc de **code***. Les accolades sont optionnelles en cas de simple variable:

```typst
This is #name's documentation
#let my-add(x, y) = x + y
Sum is #my-add(2, 3).
```
    
* `#[...]` est un *bloc de **contenu***. Il faut voir les crochets comme un délimiteur de chaîne de caractère, inspecté par `typst`.


# Points communs avec `markdown`

Il faut oublier le `#` et remplacer par `=`, sinon :

* items -> -
* `*...*` -> _..._
* backquotes ok `...` et ```bash ``` 


# Instructions notables

* #raw() -> `\begin{verbatim}`

      #raw("fn " + "main() {}", lang: "rust")

* `#v()` -> `vspace{}`
* `#sub` et `#super`
* `#hide` -> commentaire (en sus du commentaire de code // et /*...*/)

Sinon tout est disponible pour composer un document de façon programmatique.

# Introspection

`typst` permet aux partie d'un document d'interagir entre elles. Il s'agit des références de type compteur ou label de LaTeX mais permet également de requêter sur le résultat.

# Les possibilités de `typst` en une page

```typst


```


# Programmation dans un langage de composition

Ce chapitre introduit l’aspect *langage de programmation* du système. On y présente les concepts classiques — variables, tests, itérations, fonctions, interfaces — tels qu’ils sont intégrés nativement à l’écriture de documents. L’objectif est de comprendre comment la logique et le contenu coexistent de manière fluide.

## (De)-structuration de la donnée 

#let (x, y) = (1, 2)
The coordinates are #x, #y.

#let (a, .., b) = (1, 2, 3, 4)
The first element is #a.
The last element is #b.

#let books = (
  Shakespeare: "Hamlet",
  Homer: "The Odyssey",
  Austen: "Persuasion",
)

#let (Austen,) = books
Austen wrote #Austen.

#let (Homer: h) = books
Homer wrote #h.

#let (Homer, ..other) = books
#for (author, title) in other [
  #author wrote #title.
]

* typage implicite
* valeurs immuables par défaut
* portée lexicale (scope)

## Tests conditionnels

Les structures conditionnelles permettent de produire du contenu différent selon des valeurs.

`typst` propose une vision à la fois fonctionnelle et itérative du document :

```typst
#let fiche_etudiant nom note {
  #heading(level: 2)[#{nom}]
  Note finale : #{note}
}


#for (author, title) in other [
  #author wrote #title.
]
```

# Fonctions :

* abstraction
* factorisation
* construction de templates paramétrables

Une fonction peut produire une valeur ou du contenu formaté.

Une fonction prend "des" paramètres entre parenthèses s'il y en a plusieurs, sinon les crochets peuvent suffire si le paramètre attendu est un texte. Les paramètres sont la plupart du temps des chaînes de caractères et doivent donc être mises entre crochets. Si le paramètre est le résultat d'un code, il faut l'insérer entre crochets et prévenir avec un dièse.

## Interfaces et contrats d’usage

Une interface n’est pas une notion formelle, mais un *contrat implicite* entre :

* des paramètres attendus,
* un rendu produit,
* un usage prévu dans le document.

Exemple de fonction-interface :

```typst
#let fiche(nom, note) = {
  heading([Fiche de #nom])
  [Note finale #nom: #{note}]
}
#fiche("toto", 12)
```

Ici :

* `nom` et `note` constituent l’interface
* la fonction définit un format stable
* l’appelant n’a pas à connaître l’implémentation interne

---

## Modules

Modules

You can split up your Typst projects into multiple files called modules. A module can refer to the content and definitions of another module in multiple ways:

* `include "bar.typ"` : evaluates the file at the path bar.typ and returns the resulting content.
*  `import "bar.typ"` : evaluates the file at the path bar.typ and inserts the resulting module into the current scope as bar (filename without extension). You can use the as keyword to rename the imported module: import "bar.typ" as baz. You can import nested items using dot notation: import "bar.typ": baz.a.
* `import "bar.typ": a, b` : evaluates the file at the path bar.typ, extracts the values of the variables a and b (that need to be defined in bar.typ, e.g. through let bindings) and defines them in the current file. Replacing a, b with * loads all variables defined in a module. You can use the as keyword to rename the individual items: import "bar.typ": a as one, b as two


## Points clés à retenir

* le langage est évalué lors de la composition du document
* le code produit directement du contenu
* les concepts classiques de programmation sont présents, mais adaptés à un contexte déclaratif
* la frontière entre données, logique et rendu est volontairement mince

Ce socle permet d’aborder ensuite la mise en page, les styles et la construction de templates complexes sans changer de paradigme.


## Trois modes d'écriture 

1. Markup, pour le texte et la structure typographique,
1. Math, pour écrire des formules avec $ ... $,
1. Code, activé avec #, pour faire des calculs, définir des fonctions, des conditions, etc.



Element	LaTeX	Typst	See
Strong emphasis	\textbf{strong}	*strong*	strong
Emphasis	\emph{emphasis}	_emphasis_	emph
Link	\url{https://typst.app}	https://typst.app/	link
Label	\label{intro}	<intro>	label
Reference	\ref{intro}	@intro	ref
Citation	\cite{humphrey97}	@humphrey97	cite
Monospace (typewriter)	\texttt{mono}	text or mono functions	text, mono
Code	lstlisting environment	`print(f"{x}")`	raw
Verbatim	verbatim environment	`#typst-code()`	raw
Bullet list	itemize environment	- List	list
Numbered list	enumerate environment	+ List	enum
Term list	description environment	/ Term: List	terms
Figure	figure environment	figure function	figure
Table	table environment	table function	table
Equation	$x$, align / equation environments	$x$, $ x = y $	equation


#emph

* fonctions
* paramètres entre crochets
* blocks de code { let x = 1; x + 2 }
* blocks de contenu [[*Hey* there!]

* contexte : exécution dynmaique selon le contexte

```typ
#let value = context text.lang
#value

#set text(lang: "de")
#value

#set text(lang: "fr")
#value
----------------------------
#set heading(numbering: "1.")

= Introduction
#lorem(5)

#context counter(heading).get()

= Background
#lorem(5)

#context counter(heading).get()
```


* argument sinks

```typ
#let format(title, ..authors) = {
  let by = authors
    .pos()
    .join(", ", last: " and ")

  [*#title* \ _Written by #by;_]
}

#format("ArtosFlow", "Jane", "Joe")
```

* fonctions

```typ
#let alert(body, fill: red) = {
  set text(white)
  set align(center)
  rect(
    fill: fill,
    inset: 8pt,
    radius: 4pt,
    [*Warning:\ #body*],
  )
}

#alert[
  Danger is imminent!
]

#alert(fill: blue)[
  KEEP OFF TRACKS
]
```
