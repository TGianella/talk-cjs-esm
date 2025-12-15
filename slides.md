---
# try also 'default' to start simple
theme: ./theme
fonts:
  sans: Momo Trust Sans
# some information about your slides (markdown enabled)
title: CJS, ESM, WTF ??
# apply UnoCSS classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# duration of the presentation
duration: 50min
---

# CJS, ESM, WTF ??

<!-- La modularité en javascript -->

<div class="absolute top-30px left-30px flex flex-col gap-1">
  <img src='./assets/JS_lego.png' width="85" />
  <span class="text-sm font-bold">16/01/2026</span>
</div>

<div class="absolute left-0 bg-[url(./assets/lego_computer_3-1.png)] w-full h-[33vw] bg-contain bg-no-repeat"></div>
<div class="absolute font-bold bottom-16px right-30px flex gap-5 items-end">
  <span class="pb-1">9+</span>
  <span class="pb-1">Théo Gianella</span>
  <img src="./assets/logo_lyon_js.png" width=50/>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# {{ $page - 1 }} &nbsp;&nbsp;&nbsp;Faire une page web <v-click> en 2004 </v-click>

<div v-click="2" class="flex bg-[#ffffde] border border-black rounded-lg w-fit h-sm overflow-clip m-auto mt-10">
  <img v-click="2" src="./assets/instructions/1-1.png" />
  <img v-click="3" src="./assets/instructions/1-2.png" />
  <img v-click="4" src="./assets/instructions/1-3.png" />
</div>

<!--
Une page web se compose de trois éléments basiques: du contenu HTML, un style en CSS et de l'interactivité avec Javascript (on peut se rappeler qu'à l'époque beaucoup de features de HTML/CSS ne sont pas implémentées et doivent être faites en JS)
-->

---
layout: two-cols-header
---

# {{ $page - 1 }} &nbsp;&nbsp;&nbsp;Java-SCRIPTS

::left::

<div class="h-full relative">
  <img v-click.hide="1" src="./assets/instructions/2-1.png" width=100 class="absolute inset-0 m-auto" />
  <img v-click="[1, 2]" src="./assets/instructions/2-2.png" width=300 class="absolute inset-0 m-auto"  />
  <img v-click="2" src="./assets/instructions/2-3.png" width=300 class="absolute inset-0 m-auto"  />
</div>

::right::

<div class="h-full relative">
<div v-click.hide="1" class="absolute inset-0 m-auto size-fit">
```html
<script src="myScript.js"></script>
```

</div>
<div v-click="[1, 2]" class="absolute inset-0 m-auto size-fit">
```html
<script src="myHugeScript.js"></script>
```

</div>
<div v-click="2" class="absolute inset-0 m-auto size-fit">
```html
<script src="utils.js"></script>
<script src="validation.js"></script>
<script src="ajax.js"></script>
<script src="main.js"></script>
```

</div>
</div>

<!-- Le code JS est intégré aux pages HTML via le tag script. Les développeurs ont le choix de tout mettre dans un seul script énorme ou de diviser en plusieurs scripts (donc autant de balises). (sans parler des scripts tiers qui fonctionnent exactement de la même manière). Script inline ou fichier dédié, c'est exactement la même chose. Insister sur le fait que c'est via le document HTML que le javascript est parsé et exécuté. C'est dans le contexte HTML (document, navigateur) que le javascript est exécuté et donc que les scripts peuvent interagir entre eux. A cette époque on fait peu de javascript donc c'est pas un problème -->

---
layout: two-cols-header
---

# {{ $page - 1 }} &nbsp;&nbsp;&nbsp;Scripts et modularité

<!-- Amélioration possible: faire une fiche avec des étapes 1/2 pour illustrer l'ordre de chargement de main.js et utils.js -->

::left::
<div class="h-full relative">
  <img v-click="[1, 2]" src="./assets/instructions/3-1.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="[2, 3]" src="./assets/instructions/3-2.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="[3, 4]" src="./assets/instructions/3-3.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="4" src="./assets/instructions/3-3.png" width=400 class="absolute inset-0 m-auto"  />
</div>

::right::

<div class="h-full relative">
<div v-click="[2, 3]" class="absolute inset-0 m-auto size-fit">
```html
<script src="utils.js"></script>
<script src="main.js"></script>
...
```

```js
// utils.js
function formatDate(date) {
  return date.toISOString().split('T')[0];
}

// main.js
console.log(formatDate(new Date()));
```

</div>
<div v-click="[3, 4]" class="absolute inset-0 m-auto size-fit">
```html
<script src="utils.js"></script>
```

</div>
<div v-click="4" class="absolute inset-0 m-auto size-fit">
```html
<script src="main.js"></script>
<script src="utils.js"></script>
...
```

```js
// main.js
console.log(formatDate(new Date()));
// Uncaught ReferenceError: formatDate is not defined

// utils.js
function formatDate(date) {
  return date.toISOString().split('T')[0];
}
```

</div>
</div>

<!-- Parler de ce qui se passe quand on combine des scripts: objet window global (dépendances implicites) et ordre de chargement qui compte, side effects et surtout collision car pas de namespace (t'es jamais sûr de pas écraser une variable définie ailleurs). En fait c'est comme tout concaténer dans un seul script -->

---
layout: two-cols-header
---

# {{ $page - 1 }} &nbsp;&nbsp;&nbsp; Namespacing et IIFEs

::left::
<div v-click="1" class="flex flex-col bg-[#ffffde] border border-black rounded-lg h-fit w-fit overflow-clip m-auto mt-10">
 <img v-click="1" src="./assets/instructions/4-1.png" width=200 class="m-5 mb-3">
 <img v-click="2" src="./assets/instructions/4-2.png" width=200 class="m-5 mt-0">
</div>

::right::

<div class="h-full relative">
<div v-click="1" class="absolute inset-0 m-auto mt-10 w-17/20">

```js
// utils.js
var MyApp = MyApp || {};

MyApp.utils = (function() {
  var myPrivateVar = {};

  return {
    formatDate: function(date) {
      return date.toISOString();
    }
  };
})();
```

</div>
<div v-click="2" class="absolute inset-0 m-auto mt-10 w-17/20">

```js
// utils.js
var MyApp = MyApp || {};

MyApp.utils = (function() {
  var myPrivateVar = {};

  return {
    formatDate: function(date) {
      return date.toISOString();
    }
  };
})();

// main.js
console.log(MyApp.utils.formatDate(new Date()));
```

</div>
</div>



<!-- Solution: des patterns émergent pour créer de la modularité en pratique: n'exposer que les parties dont on a besoin publiquement et garder les détails d'implémentation -->

---
layout: two-cols-header
---

# {{ $page - 1 }} &nbsp;&nbsp;&nbsp;De l'autre côté du miroir

::left::
<div class="h-full relative">
  <img class="absolute inset-0 m-auto" v-click="[1, 3]" src="./assets/instructions/laptop.png" width=300>
  <img class="absolute inset-0 m-auto" v-click="4" src="./assets/instructions/5-1.png" width=300>
</div>

::right::

<div class="h-full relative">
  <img class="absolute inset-0 m-auto" v-click="[2, 3]" src="./assets/instructions/server.png" width=200>
  <img class="absolute inset-0 m-auto" v-click="5" src="./assets/instructions/5-2.png" width=300>
</div>

<!-- Bilan: Tout ce qu'on a vu, c'est le standard de fonctionnement de JS dans un navigateur, et ça fonctionnait. Mais à la fin des années 2000, Javascript va profondément se transformer en devenant un langage utilisable côté serveur (attention, il y a eu des runtimes serveurs depuis le milieu des années 90 (netscape) mais jamais vraiment d'essor.). Ce qui se passe à la fin des années 2000 de décisif: sortie de V8 en septembre 2008, un moteur JavaScript très performant, conçu pour être embeddable (pas lié à un navigateur) et open-source. Node sort en mai 2009, basé sur V8, wrapper autour de V8 avec de l'I/O asynchrone et une event loop. Donc fin des années 2000 - début des années 2010, on a une stack technique qui se met en place pour faire du JS côté serveur. -->

---
transition: fade
---

# {{ $page - 1 }} &nbsp;&nbsp;&nbsp;Un écosystème JavaScript côté-serveur ?

<div class="h-full grid place-items-center">
  <img v-click src="./assets/instructions/5-3.png" width=200>
</div>



<!-- Fin des années 2000: moment d'effervescence autour de JavaScript. On a parlé des aspects techniques, mais ce n'est pas tout. Au même moment, va se mettre en place un vrai écosystème JavaScript serveur. Kevin Dangoor un développeur de Mozilla publie un post sur son blog qui va avoir un grand retentissement: il dit dedans que le JS côté serveur n'a pas tant besoin de solutions techniques (il y a en a plein qui existent déjà), mais d'un écosystème: une lib standard, des interfaces standard,  un certain nombre de conventions, un moyen unique et simple de publier et d'installer des dépendances en cross-platform, et... une méthode standard pour inclure des modules et les scoper proprement.-->

---
layout: quote
---

<div class="text-4xl/10">JavaScript needs a <span class="font-bold">standard way to include other modules</span> and for those modules to live in discreet namespaces. There are easy ways to do namespaces, but there’s no standard programmatic way to load a module (once!). This is really important, because server side apps can include a lot of code (...).</div>

<!-- Le JS côté serveur a vraiment besoin d'un système de modules car il y aura beaucoup plus de code avec un usage plus général. K Dangoor crée un groupe de discussion nommé ServerJS, qui sera renommé rapidement CommonJS. Attention CommonJS ne fait partie du TC39, elle ne définit pas une spécification officielle de JavaScript. Une spécification pour un système de module est rapidement établie et adoptée par NodeJS.-->

---
layout: two-cols-header
---

# {{ $page - 2 }} &nbsp;&nbsp;&nbsp; Les modules CommonJS

<!-- Idée: parler des propositions de spec alternatives aux secured modules -->

::left::

<div class="h-full relative">
  <img v-click="[1, 3]" src="./assets/instructions/9-6.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="[3, 5]" src="./assets/instructions/9-2.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="[5, 6]" src="./assets/instructions/9-3.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="[6, 7]" src="./assets/instructions/9-4.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="[7, 8]" src="./assets/instructions/9-5.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="[8, 9]" src="./assets/instructions/9-6.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="9" src="./assets/instructions/9-7.png" width=400 class="absolute inset-0 m-auto"  />
</div>

::right::

<div class="h-full relative">
<div v-click="[1, 2]" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// main.js
(...)
```

</div>
<div v-click="[2, 3]" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// main.js
(...)

var module = require('./module');
```

</div>
<div v-click="[3, 4]" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// module.js 
var private = "not exported";
console.log(private);
```

</div>
<div v-click="[4, 5]" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// module.js 
var private = "not exported";
console.log(private);
```

```
"not exported"
```

</div>
<div v-click="[5, 6]" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// module.js
var private = "not exported";
console.log(private);

exports.foo = "foo";
```

</div>
<div v-click="[6, 7]" class="absolute inset-0 m-auto size-fit w-17/20">

```js
// module.js
var private = "not exported";
console.log(private);

exports.foo = "foo";
exports.bar = "bar";
```

</div>
<div v-click="[7, 8]" class="absolute inset-0 m-auto size-fit w-17/20">

```js
// module.js
var private = "not exported";
console.log(private);

exports.foo = "foo";
exports.bar = "bar";
exports.baz = "baz";
```

</div>
<div v-click="[8, 9]" class="absolute inset-0 m-auto size-fit w-17/20">

```js
// main.js
(...)

var module = require('./module');
```

</div>
<div v-click="9" class="absolute inset-0 m-auto size-fit w-17/20">

```js
// main.js
(...)

var module = require('./module');

var foo = module.foo; // 'foo'
var bar = module.bar; // 'bar'
var baz = module.baz; // 'baz'
```

</div>
</div>


<!-- La spec définit de manière simple comment on exporte et importe des modules. Chaque module a accès à une fonction globale, require, qui prend en argument un identifiant de module et exécute le code du module pour obtenir son API exportée. Au moment où on exécute le require, les exports du module sont inconnus, il faut résoudre. Pour cela chaque module a un objet exports auquel on peut donner des propriétés (on ne peut pas le réassigner).

La spec est pensée pour le serveur: tous les fichiers sont sur le même filesystem donc le coût de chargement d'un module est négligeable. Modèle mental assez simple. On require, on exécute et on retourne les exports. Tout se fait au runtime, les imports et exports sont des instructions (appel de fonction et assignation à un objet). Architecture require-first.

Comme on exécute les modules pour définir leur imports, attention aux side-effects (mais ça peut être utile).
 -->

---
layout: two-cols-header
---

# {{ $page - 2 }} &nbsp;&nbsp;&nbsp;Dépendances circulaires

::left::

<div class="h-full relative">
<div v-click="[1, 4]" class="absolute inset-0 m-auto size-fit w-17/20">

```js
//a.js
var b = require('b.js');
```

</div>
<div v-click="[4, 5]" class="absolute inset-0 m-auto size-fit w-17/20">

```js
//a.js
exports.foo = "foo";

var b = require('b.js');
```

</div>
<div v-click="5" class="absolute inset-0 m-auto size-fit w-17/20">

```js
//a.js
exports.foo = "foo";

var b = require('b.js');

exports.bar = "bar";
```

</div>
</div>

::right::

<div class="h-full relative">
<div v-click="[1, 2]" class="absolute inset-0 m-auto size-fit w-17/20">

```js
//b.js
var a = require('a.js');
```

</div>
<div v-click="[2, 3]" class="absolute inset-0 m-auto size-fit w-17/20">

```js
//b.js
var a = require('a.js');

console.log(a.foo)
```

</div>
<div v-click="[3, 4]" class="absolute inset-0 m-auto size-fit w-17/20">

```js
//b.js
var a = require('a.js');

console.log(a.foo) // undefined
```

</div>
<div v-click="[4, 6]" class="absolute inset-0 m-auto size-fit w-17/20">

```js
//b.js
var a = require('a.js');

console.log(a.foo) // 'foo'
```

</div>
<div v-click="6" class="absolute inset-0 m-auto size-fit w-17/20">

```js
//b.js
var a = require('a.js');

console.log(a.foo) // 'foo'
console.log(a.bar) // undefined
```

</div>
</div>

<!-- Les dépendances circulaires sont gérées en utilisant la valeur d'exports au moment où la dépendance circulaire commence. C'est-à-dire que quand on require un module, s'il a déjà été (même partiellement !) exécuté, il n'est pas exécuté à nouveau mais on utilise la valeur de son objet exports (qui pourra changer si l'exécution reprend après la résolution des dépendances). 

Plusieurs conséquences: 
* Les dépendances circulaires fonctionnent
* Attention à l'ordre de chargement des modules, il faut faire gaffe
* Il y a donc un cache de modules: chaque module est chargé une seule fois (au total, ça peut se faire en plusieurs fois si on est interrompu par un require). -->



---

# {{ $page - 2 }} &nbsp;&nbsp;&nbsp; De CommonJS à Node.js

<div class="h-full relative">
  <img v-click="[1, 6]" src="./assets/instructions/6-1.png" width=200 class="absolute right-150 inset-0 m-auto">
  <img v-click="[2, 6]" src="./assets/ringo-mascot.svg" width=100 class="absolute bottom-80 right-75 inset-0 m-auto">
  <img v-click="[3, 6]" src="./assets/instructions/6-2.png" width=100 class="absolute bottom-80 left-75 inset-0 m-auto">
  <img v-click="[4, 6]" src="./assets/instructions/6-3.png" width=100 class="absolute left-150 inset-0 m-auto">
  <img v-click="[5, 7]" src="./assets/instructions/5-2.png" width=200 class="absolute inset-0 m-auto">
  <img v-click="7" src="./assets/instructions/10-1.png" width=150 class="absolute inset-0 right-100 m-auto">
  <img v-click="7"src="./assets/instructions/10-2.png" width=180 class="absolute inset-0 left-100 m-auto">
</div>

<!-- A cette période, Node.js n'est qu'un runtime JS serveur parmi d'autres (on a des outils il manquait un écosystème). Tous sont liés de près où de loin à CommonJS. Au début des années 2010, node devient le standard de facto du JS côté serveur et les alternatives ne décollent jamais (narwhal, ringoJS, v8cgi (teaJS), GPSEE). 

Ca créé des tensions dans la communauté (entre les contributeurs commonJS qui essayent d'élaborer une spec et n'implémentent rien et nodeJS). Le truc intéressant: node a implémenté les modules de commonJS, juste pas les autres trucs, mais Node est devenu le standard donc ça fait survivre les modules et on a oublié tout le reste de ce qui a été proposé par commonJS. Consensus: CommonJS était un projet trop ambitieux, mais ça a permis de faire node qui est devenu le standard côté serveur, donc un standard a bien émergé et tout le monde est content.  -->

---
layout: two-cols-header
---

# {{ $page - 2 }} &nbsp;&nbsp;&nbsp; Les modules Node.js: le module wrapper

::left::

<div class="h-full relative">
<div v-click="[1, 2]" class="absolute inset-0 m-auto size-fit">

```js
(function(exports, require, module, __filename, __dirname) {
// Module code actually lives in here
}); 
```

</div>
<div v-click="[2, 3]" class="absolute inset-0 m-auto size-fit w-25/20">

```js
//main.js

```

</div>
<div v-click="[3, 4]" class="absolute inset-0 m-auto size-fit w-25/20">

```js
//main.js
var nestedModule = require('./subfolder/nestedModule.js');
```

</div>
<div v-click="[4, 5]" class="absolute inset-0 m-auto size-fit w-25/20">

```js
//main.js
var nestedModule = require('./a/module.js');

console.log(nestedModule.require === require); // false

```

</div>
<div v-click="5" class="absolute inset-0 m-auto size-fit">

```js
//main.js
var nestedModule = require('./a/module.js');

console.log(nestedModule.require === require); // false

var sameLevelModule = nestedModule.require('./sameLevelModule.js');
// Error: Cannot find module './sameLevelModule.js'
```

</div>
</div>

::right::

<div class="h-full relative">
<div v-click="[2, 4]" class="absolute inset-0 m-auto size-fit">

```
.
├── main.js
├── sameLevelModule.js
└── subfolder/
    └── nestedModule.js
```

</div>
<div v-click="[4, 5]" class="absolute inset-0 m-auto size-fit">

```js
//nestedModule.js
exports.require = require;
```

</div>
<div v-click="5" class="absolute inset-0 m-auto size-fit">

```
.
├── main.js
├── sameLevelModule.js
└── subfolder/
    └── nestedModule.js
```

</div>
</div>

 <!-- Spécificités de l'implémentation de node. 
Module wrapper: à l'exécution, Node wrappe le module dans une fonction dans laquelle il injecte 5 paramètres, exports, require, module, __filename et __dirname. Ca permet d'avoir des variables "globales" (on ne les importe jamais manuellement) mais en fait propres à chaque module. Notamment la fonction require est unique pour chaque fonction
Objet module. -->

---
layout: two-cols-header
---

# {{ $page - 2 }} &nbsp;&nbsp;&nbsp; L'objet module

::left::

<div class="h-full relative">
  <img v-click="[1, 2]" src="./assets/instructions/9-5.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="[2, 3]" src="./assets/instructions/13-1.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="[3, 5]" src="./assets/instructions/13-2.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="5" src="./assets/instructions/13-1.png" width=400 class="absolute inset-0 m-auto"  />
</div>

::right::

<div class="h-full relative">
<div v-click="[1, 2]" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// module.js
exports.foo = "foo";
exports.bar = "bar";
exports.baz = "baz";
```

</div>
<div v-click="[2, 4]" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// module.js
(...)

exports = new MyClass();
```

</div>
<div v-click="[4, 5]" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// main.js
var MyClass = require('./module.js'); // {}
```

</div>
<div v-click="5 " class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// module.js
(...)

module.exports = new MyClass();
```

</div>
</div>

<!-- Jusqu'ici on a uniquement exporté via exports, comme d'après la spec CommonJS. Mais c'est pas la façon habituelle de faire, on utilise module.exports. Simple question d'assignation, si on veut retourner une seule valeur (genre une instance de classe), on ne peut pas réassigner l'objet exports (on change la référence, ça devient un objet local au module) -->

---
layout: two-cols-header
---

# {{ $page - 2 }} &nbsp;&nbsp;&nbsp; Manipulation de modules

::left::

<div class="h-full relative">
  <img v-click.hide="1" src="./assets/instructions/13-1.png" width=400 class="absolute inset-0 m-auto"  />
  <img v-click="[1, 4]" src="./assets/instructions/14-1.png" width=200 class="absolute inset-0 m-auto"  />
  <img v-click="4" src="./assets/instructions/14-2.png" width=234 class="absolute inset-0 right-8 m-auto"  />
</div>

::right::

<div class="h-full relative">
<div v-click.hide="2" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// module.js
var foo = function () {
  console.log('foo');
}

exports.foo = foo;
```

</div>
<div v-click="[2, 3]" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// main.js
var module = require('./module.js');
var module2 = require('./module.js');
```

</div>
<div v-click="[3, 4]" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// main.js
var module = require('./module.js');
var module2 = require('./module.js');

console.log(module === module2); // true
```

</div>
<div v-click="[4, 5]" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// main.js
var module = require('./module.js');
module.foo(); // 'foo'

module.foo = function() {
  console.log('bar');
}
module.foo(); // 'bar'
```

</div>
<div v-click="5" class="absolute top-0 left-1/2 -translate-x-1/2 w-17/20">

```js
// main.js
var module = require('./module.js');
module.foo(); // 'foo'

module.foo = function() {
  console.log('bar');
}
module.foo(); // 'bar'

var module2 = require('./module.js');
module2.foo(); // 'bar'
```

</div>
</div>

<!-- Ajouter une slide sur le cache, sur le fait que chaque module est instancié une seule fois, et que du coup un module peut en modifier un autre et la modification est active pour tous les autres consommateurs. -->

<!-- Recap CJS:
* Un système synchrone (chaque module est chargé quand il est requis)
* Tout se fait au runtime (require est une fonction JS qu'on appelle, on ne connaît pas les exports à l'avance) 
* Un système pensé pour le serveur
* Implémentation par des runtimes, pas une feature de JS (wrapper les modules)
-->

<!-- Est-ce que ça a des influences côté-client ? -->

