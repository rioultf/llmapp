V---
author: François Rioult
title: Observable
---

*Les exemples présentés ici sont issus de *notebooks* et le code présenté ne doit donc pas être considéré sur plusieurs lignes mais sur plusieurs **cellules**.*

# Introduction 

Par Mike Bostock, le père de D3.js, Observable propose la rédaction de *notebooks* mélangeant `markdown` et Javascript. Le notebook fonctionne comme un tableur, composé de cellules, représentée par une variable JS. Des *générateurs* ou des *inputs* permettent d'animer le tout.

J'ai regardé il y a 3 ans, en fan de D3.js -- et puis Observable contient la documentation de D3.js -- mais je n'avais rien compris. C'est vrai qu'il faut creuser avant de comprendre l'évidence.

Ce truc est absolument incroyable. On définit en très peu de code :
* les entrées
  * data : fichiers, base de données
  * `Inputs` : slider, checkbox, etc.
  * générateurs : un simple `await i` capte la modification de `i`
  * transformation
  * génération d'éléments `Plot`

  Un notebook `Observable` fonctionne comme un tableur, composé de cellules connectées par un graphe de dépendance. Ce n'est pas un notebook qui exécute des instructions séquentielles. Il est essentiel de bien différencier les *mutable*, modifiés par les éléments d'UI, des variables calculées selon des dépendances. Pour bien se représenter ces dernières, le panneau `minimap` permet la navigation parmi les cellules, représentées selon leur nature : source, puits, intermédiaire, fonction.

  Développer en `Observable` est exigeant. Même si on peut enchaîner les cellules, on arrive rapidement à une organisation plus proche de la pile : il faut coder à l'envers et annexer ce qui fonctionne. L'usage de la minimap est d'une grande aide.

  Pour renommer une variable, utiliser le panneau rechercher/remplacer.

## Trigger personnalisés

Pour induire une relation dans le graphe de dépendance, il suffit de *l'indiquer* lors de la définition de la cellule\ :

```js

viewof reset  = Inputs.button("Reset", {value: 0})
elapsed = {reset;
  let t = 0;
  while (true) {
    yield t++;
    await Promises.tick(1000);
  }
}
```

<https://observablehq.com/@observablehq/introduction-to-asynchronous-iteration>

D3.js est une librairie JS qui transforme des données structurées en HTML dynamique, avec des effects d'animation. La documentation est ardue, il faut travailler D3 à partir d'exemples avant de bien comprendre les mécanismes de sélection. Après, c'est du HTML brut.

D3.js devient une librairie de manipulation de données pour Observable, qui est un agrégateur de mardown, JS et librairie de visualisation, comme D3, mais propose une version simplfiée, bien plus orientée sur les concepts de la dataviz.

La documentation d'Observable est meilleure que celle de D3.js. Toujours beaucoup d'exemple, mais le côté notebook transcende l'apprentissage. L'interactivité est incroyablement facile à programmer, après près de 40 années d'enseignement des maths au tableau, je trouve enfin l'outil ultime de tous mes délires. Et les cellules de notebook ont une URL partageables dans d'autres notebooks.

De nombreux enseignants s'en sont emparé, alors que D3.js est plus réservé aux codeurs.

Au lieu d'une intégration dans une UI en HTML, Observable permet le test/déploiement sous forme de notebook puis génération de l'intégration sous forme de fonction JS associées aux cellules, qu'il suffit d'intégrer dans un 

# Philosophie

The Observable runtime listens for input events on the view, and doesn’t check whether the value of the view has changed

Un `Plot.<marker>(...)` est d'abord construit sur une source de données à laquelle est appliquée une transformation. La source peut être le résultat d'un InputLa transformation est un dictionnaire de correspondance entre des attributs standards, x, y, etc. et les attributs correspondants dans les données. 

Function attributes : If an attribute value is a function, it is assigned as a property. This can be used to register event listeners.


## Mutables

Normally, only the code block that declares a top-level variable can define it or assign to it. You can however use the Mutable function to declare a mutable generator, allowing other code to mutate the generator’s value. This is similar to React’s useState hook.

https://observablehq.com/@kelleyvanevert/settable-variables/2
Use ⟨var⟩.set(..) to set its value anywhere, and v = Generators.input(⟨var⟩) in another cell, to use it.

## Hypertext Literal

C’est le nom d’une fonction/utilitaire utilisé dans ObservableHQ pour écrire du HTML dans du JavaScript de façon sûre et flexible.

une La phrase décrit htl (Hypertext Literal), une fonction JavaScript qui sert de tagged template literal pour générer du HTML de façon sûre et flexible.
Elle met en avant trois points principaux :

    l’origine (inspirations techniques) ;

    le comportement (interpolation selon le contexte) ;

    les capacités (échappement automatique et insertion de valeurs non sérialisables).

une tagged template literal est une fonction qui prend une chaîne littérale (template) et ses expressions interpolées pour produire une sortie structurée (ici, du DOM)

l’insertion des valeurs dans le template ne se fait pas naïvement par concaténation de chaînes, mais en tenant compte de où la valeur est insérée (dans un attribut, dans du texte, etc.).

## Orchestration

> il faut raisonner en termes de cellules qui lisent et d'inputs qui *écrivent* (déclenchement d'événement). Une variable peut être *mutable* mais pas un Inputs, qui n'est mutable qu'en interne.

### Minimiser les mutations

> cell evaluation order doesn't matter

> si l'on pense que le code est exécuté linéairement, on commet des erreurs et on obtient des résultats incohérents (race condition). Il faudrait même s'habituer à écrire le code à l'envers.

[Il est important de minimiser les *mutations*](https://observablehq.com/d/13d50ff97dd5544c). À la recherche de dépendances entre cellules, le notebook doit pouvoir clairement les identifier.

En particulier, il ne faut pas définir de mutations (ex. \.splice() et .sort() dans les cellules. Si elles ne définissent pas de dépendances, elles seront exécutées une seule fois, **mais** l'exécution est incrémentale :

```js
// run this cell
a = [1, 2, 3, 4]
// then run this cell a few times
a.splice(0, 1)
// Then run this one
a.length
```

On peut finir avec zéro !

#### Minimiser les mutations, maximiser les dépendances.

* Using an alternative that doesn't mutate the data - for instance, using Array#concat, which returns a new Array, instead of Array#push, which modifies an array in-place
* Making a copy of the data and mutating that: for instance, by using Array#slice()

Voici comment il faudrait procéder :

```js
c = {
  let copy = b.slice();
  copy.splice(0, 1);
  return copy;
}
```

De même, il ne faut pas muter le composant d'un objet :

```js
myObject = ({hi:'Tom'})
// mutation par ajout d'attribut
myObject.key = Date.now()

// définition d'une dépendance à myO
myObjectWithKey = ({ key: Date.now(), ...myObject })

```




```js
// ne pas faire
html`<div id='my-fun-element'></div>`
// provoque une erreur car on ne sait pas dans quel ordre sont executées les lignes.
document.querySelector('#my-fun-element').innerText = 'What a fun element!'

myElement = html`<div></div>`
```



* [Source : Observable anti-patterns and code smells](https://observablehq.com/d/13d50ff97dd5544c)


    Timers: use them sparingly, a lot of times there’s a better way
    Mutation: try to avoid it!
    Generators: the return value doesn’t matter
    Re-selecting elements: it's much better to reference elements in a notebook using variables than it is to reference them using class or tag names.


### Mutations autorisées

Pour conclure, on pourra utiliser le reducer d'un Inputs pour déclencher une mutation :


```js
mutable objToReplace = ({ x: 1, y: 1});
viewof replace = Inputs.button("replace", {reduce: () => mutable objToReplace = { ...objToReplace, x: objToReplace.x + 1 }})
```




[sur la différence entre `viewof` et `mutable`](https://talk.observablehq.com/t/a-bit-confused-about-mutable-etc/721/2)

Il faut maintenir une séparation entre les cellules qui lisent ou écrivent. Pas les deux en même temps.

### `viewof` vs `mutable` 

`viewof` est l'essentiel accès en lecture et doit préfixer l'emploi d'une variable cellule dans les éléments de rendu, par exemple : 

    ${viewof hatch}${viewof hatch.value}

Selon les cas, on accèdera donc à un Input pour sa forme de widget ou pour sa valeur, mais toujours précédé de `viewof`.

`mutable` est utilisé pour définir des variables sur lesquelles le code peut interagir, à réserver pour les accès en écriture.

Ces mots clés signalent au framework les relations de dépendance entre les cellules. Dans le graphe de dépendance, les `viewof` sont des puits, les `mutable` sont des sources.


`viewof` et `mutable` sont du sucre syntaxique :

* `viewof` crée deux variables :

```js
viewof foo = html`<input type="range">`
// is equivalent to
viewof_foo = html`<input type="range">`
foo = Generators.input(viewof_foo)
```

  Toute référence à `viewof foo` est équivalente à `viewof_foo`. `foo` reste la valeur.

* `mutable`:

```js
mutable foo = 0
// is equivalent to
initial_foo = 0
mutable_foo = new Mutable(initial_foo)
foo = mutable_foo.generator
```

Tout accès en écriture doit utiliser `mutable foo`, équivalent de `mutable_foo.value`.


```js
// définition d'une variable mutable (par défaut, elle seule peu se modifier)
mutable clicks = 0
// formulaire HTML exclusivement (par de Inputs.button())
<button onclick=${() => ++mutable clicks}>click me</button>
viewof reset = Inputs.button([["Reset", value => {mutable clicks = 0; return value + 1}]], {value: 0})
```

* définition d'une vue `composite` d'inputs :

```js
viewof composite = view`<div style="display: flex; justify-content:space-between; ">
<div style="display: flex-column;">
  <div>${["r1", Inputs.range([0, 10])]}</div>
  <div>${["r2", Inputs.range([0, 3])]}</div>
  <div>${[
      "text",
      Inputs.text({
        label: "Enter some text"
      })
    ]}</div>
</div>
`

htl.html`<button onclick=${() => {
  viewof composite.value = {
    r1: Math.random() * 10,
    r2: Math.random() * 3,
    text: `${Math.random()}`
  };
  viewof composite.dispatchEvent(new Event('input'));
}}> randomize composite`
```

Un bouton peut désactiver un autre bouton :

```js
viewof frozen = Inputs.toggle({label: "Frozen", value: true, disabled: viewof disable})
viewof disable = Inputs.toggle('disable')
```

### Bascule R/S

viewof b = Inputs.button()
{
  b
  return !this;
}



html`<button onclick=${() => ++mutable clicks}>click me</button>`
mutable clicks = 0

<https://observablehq.com/@observablehq/hello-inputs>

```js
viewof sport = Inputs.select(athletes.map(d => d.sport), {sort: true, unique: true, label: "sport"})
selectedAthletes = athletes.filter(d => d.sport === sport)
columns = athletes.columns.slice(1, -1)
Inputs.table(selectedAthletes, {columns})
```

## Vues

<https://observablehq.com/@observablehq/views>

Une vue est l'association d'un élément interactif du DOM et d'une valeur.

* utiliser `viewof` pour déclarer dans une cellule
[viewof creates a second hidden cell foo that exposes the current value of this input element to the rest of the notebook](https://observablehq.com/@observablehq/a-brief-introduction-to-viewof)

La seconde cellule est un générateur qui yield une nouvelle valeur si l'utilisateur interagit


* on peut accéder différemment à 

```js

explicitView = Inputs.range([0,100])  // et non pas viewof
explicitValue = Generators.input(explicitView)
```

On peut accéder à la valeur d'une vue en lecture par 

```js
viewof x = Inputs.range([0,1])
md `The view of *x* is ${viewof x}.` --> affiche l'input
md `The view of *x* is ${viewof x.value}.` --> affiche la valeur mais nécessite d'émettre un input event
md `The view of *x* is ${x.value}.` --> undefined
```

Préférer :
```js
x = Inputs.range([0,1])
md `The view of *x* is ${x}.`
md `The view of *x* is ${x.value}.` --> affiche la valeur mais nécessite d'émettre un input event
xPlicit = Generators.input(x)
md `The view of *x* is ${xPlicit}.` 
```

`viewof` est dans tous les cas indispensables. En effet, `x` n'est pas une variable standard JS, c'est une vue.


# Documentation

* [Observable documentation](https://observablehq.com/documentation/)
* [Inputs](https://observablehq.com/documentation/inputs/overview)
* la documentation du framework fournit des exemple en observable JS, à coder. Pour les notebooks, il faut adapter.
* [API plot](https://observablehq.com/plot/api)
* [Librairies](https://observablehq.com/@observablehq/recommended-libraries?collection=@observablehq/libraries)
* [Plot cheatsheets](https://raw.githubusercontent.com/observablehq/plot-cheatsheets/main/plot-cheatsheets.pdf)


# Transformations

Bin
Centroid
Dodge
Filter
Group
Hexbin
Interval
Map
Normalize
Select
Shift
Sort
Stack
Tree
Window

# Iterables

https://observablehq.com/@tmcw/pure-iterables

https://observablehq.com/d/906b01641d6f7865#cell-174

```js
test = html`<h1>Test</h1><ul>${
  Array.from({length: 7}, (_, i) => html`<li> Index: ${i}</li>`)
}</ul>`
```

Routine : 

* définir des structures
* itérer en **`.map((d, i) => ...${d.id})`** non pas `.map(d => ...${d.id})` qui ne fonctionne pas.

```js
html`<table>
  <thead><tr><th>#</th><th>Color</th><th>Swatch</th></tr></thead>
  <tbody><tr>${meats.map((d, i) => html`<th>${d.id}</th>`)}</tr></tbody>
</table>`
```

## Envent listeners

https://observablehq.com/@mbostock/event-listeners


# Plot

```js
{
  {
  const x = 5
}
  //return x provoque une erreur : RuntimeError: x is not defined
  // sans valeur de retour : undefined
  return 2
}
```

https://observablehq.com/@observablehq/htl
html.fragment

# Structure d'`Observablehq`

Il faut bien identifier dans quelle partie de la documentation on se trouve :

1. notebooks avec `ObservableJS`

        viewof x = Inputs.range([0, 100])

1. Framework avec `vanillaJS`
        
        const x = view(Inputs.range([0, 100]));



A Framework project consists of a home page (index.md) and any of the following:

    Additional pages (.md)
    Data loaders (.csv.py, .json.ts, etc.)
    Static data files (.csv, .json, .parquet, etc.)
    Other static assets (.png, .css, etc.)
    Shared components (.js)
    An app configuration file (observablehq.config.js)

## Markdown 

* [référence](https://observablehq.observablehq.cloud/framework/markdown)
   
* [quickref](<https://observablehq.com/framework/markdown#basic-syntax>)

* on peut grouper des Input avec Input.form()

## HTML

html`${[viewof n, viewof a, viewof x, viewof y]}`
viewof n = Range([1, 30], {value: 19, step: 1, label: "size" })
viewof a = Range([0, 45], {value: 27.5, step: 0.1, label: "angle" })
viewof x = Range([-100, 100], {value: 0, step: 1, label: "x offset" })
viewof y = Range([-100, 100], {value: 0, step: 1, label: "y offset" })

[Fancy Input Layout](https://observablehq.com/d/6b1a61b25b3a4b7f)


Il faut utiliser HTML dès qu'on veut structurer, par exemple en colonnes.
Ne pas oublier la CSS :

        stylesheet = html`<link href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" media="all" rel="stylesheet">`

# Pièges

* viewof / pas viewof
* `x = yield { id : ..., ...}`


html`${[viewof n, viewof a, viewof x, viewof y]}`


# Générateurs :

dans les boucles, `yield` transforme une cellule en générateur. Il faut cependant écrire les boucles de façon spécifique, car Javascript n'autorise `yield` que dans une générateur

```js
foo = {
  for (let i = 0; i < 10; ++i) {
    yield i;
  }
}
```

Can be converted to:

```js
const foo = (function* () {
  for (let i = 0; i < 10; ++i) {
    yield i;
  }
})();
```

## Javascript

Les notebooks utilisent Observable JS, le framework utilise Vanilla.

## Cellules

Cells are separate scripts 
Cells run in topological order 
Cells re-run when any referenced cell changes 
Cells implicitly await promises 

```js
hello = new Promise(resolve => {
    setTimeout(() => {
        resolve("hello there")
    }, 30000)
})
```

Cells implicitly iterate over generators 
Cells can be views 

### STD Lib

* user
* connection BD
* DOM
* File attachment comme générateur
* générateurs
https://observablehq.com/documentation/misc/standard-library
    Generators.input(input)
        Returns a new generator that yields promises to the current value of the specified input element; each promise resolves when the input element emits an event. (The promise resolves when the event is emitted, even if the value of the input is unchanged.) If the initial value of the input is not undefined, the returned generator’s first yielded value is a resolved promise with the initial value of the input.
    Generators.observe(initialize)
        Returns a generator that yields promises to an observable value, adapting a push-based data source (such as an Observable, an EventEmitter or an EventTarget) to a pull-based one.

    Generators.queue(initialize) 
* graphviz
* html
* Inputs 

à la source : 
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator
Values that change over time — such as interactive inputs, animation parameters, or streaming data — can be represented in Framework as async generators. When a top-level generator is declared, code in other blocks sees the generator’s latest yielded value and runs each time the generator yields a new value.

# conversion des notebooks

JavaScript syntax

Framework uses vanilla JavaScript syntax while notebooks use a nonstandard dialect called Observable JavaScript. A JavaScript cell in a notebook is technically not a JavaScript program (i.e., a sequence of statements) but rather a cell declaration; it can be either an expression cell consisting of a single JavaScript expression (such as 1 + 2) or a block cell consisting of any number of JavaScript statements (such as console.log("hello");) surrounded by curly braces. These two forms of cell require slightly different treatment. The convert command converts both into JavaScript fenced code blocks.

un bouton est muni d'une opération `reduce` pour modifier la valeur interne :

```js
viewof time = Inputs.button("Update", {value: null, reduce: () => new Date})
const counter = view(Inputs.button([
  ["Increment", value => value + 1],
  ["Decrement", value => value - 1],
  ["Reset", value => 0]
], {value: 0, label: "Counter"}));
```


https://mauriciopoppe.github.io/function-plot/

Frames are most commonly used in conjunction with facets to provide better separation (Gestalt grouping) of faceted marks. Without a frame, it can be hard to tell where one facet ends and the next begins.

### Marks

Marks are geometric shapes 
Marks are layered 
Marks use scales 
Marks have tidy data 
Marks imply data types 
Marks have options 
Marks have channels 
Mark options 
    fill - fill color
    fillOpacity - fill opacity (a number between 0 and 1)
    stroke - stroke color
    strokeWidth - stroke width (in pixels)
    strokeOpacity - stroke opacity (a number between 0 and 1)
    strokeLinejoin - how to join lines (bevel, miter, miter-clip, or round)
    strokeLinecap - how to cap lines (butt, round, or square)
    strokeMiterlimit - to limit the length of miter joins
    strokeDasharray - a comma-separated list of dash lengths (typically in pixels)
    strokeDashoffset - the stroke dash offset (typically in pixels)
    opacity - object opacity (a number between 0 and 1)
    mixBlendMode - the blend mode (e.g., multiply)
    imageFilter - a CSS filter (e.g., blur(5px)) ^0.6.7
    shapeRendering - the shape-rendering mode (e.g., crispEdges)
    paintOrder - the paint order (e.g., stroke)
    dx - horizontal offset (in pixels; defaults to 0)
    dy - vertical offset (in pixels; defaults to 0)
    target - link target (e.g., “_blank” for a new window); for use with the href channel
    className - the class attribute, if any (defaults to null) ^0.6.16
    ariaDescription - a textual description of the mark’s contents
    ariaHidden - if true, hide this content from the accessibility tree
    pointerEvents - the pointer events (e.g., none)
    clip - whether and how to clip the mark
    tip - whether to generate an implicit pointer tip ^0.6.7


* `Plot.ruleY([0], {stroke: "red"}),`
  une ligne verticale

# Notes

* KTS stands for "Knowledge Transformation System". It is a collection of tools for creating, moving, linking, re-shaping, visualizing and exploring information, plus the fundamental methods and practices for such processes.
<https://observablehq.com/@bogo/value-maps-guide?collection=@bogo/kts-user-documentation>

* Vega-Lite is a high-level grammar of interactive graphics. It provides a concise, declarative JSON syntax to create an expressive range of visualizations for data analysis and presentation. 

* [Datavisualisation Guide](https://data.europa.eu/apps/data-visualisation-guide/)


- se pose la question de ce qui est récupéré comme dépendance lorsque qu'on écrit une fonction.

```js
// différencier
updatePopulation = ({category, type, direction}) => {
  let newPopulation = {...population}
  const prime = prime(category, type)
  newPopulation[type] -= unlocks[type]
  newPopulation[prime] += unlocks[type] / Math.pow(pows[category], state[type])
  return newPopulation  
}
mutable population = updatePopulation(population, {category, type, direction})
// Obervable détecte une dépendance à updatePopulation

// de

// dans le reduce :
mutable population = updatePopulation(population, {category, type, direction})
```

- à ajouter au bréviaire JS :

ou comment créer une donnée `{<type>: <value>}` équivaut à `[type, value]` !


```js
progressState = Object.fromEntries(
  Object.keys(unlockFunctions).map(type => {
    const current = ...
    return [
      type,
      {
        ...
      }
    ]
  })
)
```
