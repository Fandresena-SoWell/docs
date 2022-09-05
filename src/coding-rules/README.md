---
sidebar: auto
---

# Coding Rules
Ce document liste toutes les bonnes pratiques concernant l'écriture et la structuration de tout le base de code de Sowell côté front end, notamment les fichier TS, VUE et JS. 

La valeur à long terme d'un logiciel pour une organisation est directement proportionnelle à la qualité de la base de code. Au cours de sa vie, un programme sera manipulé par de nombreuses paires de mains et d'yeux. Si un programme est capable de communiquer clairement sa structure et ses caractéristiques, il est moins probable qu'il se brise lorsqu'il est modifié dans un avenir jamais trop lointain.

Les conventions de code peuvent contribuer à réduire la fragilité des programmes.

## Indentation
L'unité d'indentation est de **deux espaces**. L'utilisation d'espaces peut produire une taille de fichier plus importante, mais la taille n'est pas significative au niveau de la lecture du fichier, et la différence est éliminée par la minification.
## Longueur de ligne
Évitez les lignes de plus de 80 caractères. Lorsqu'une déclaration ne tient pas sur une seule ligne, il peut être nécessaire de la rompre. Placez la coupure après un opérateur, idéalement. La ligne suivante doit être indentée de 2 espaces de place.

```js
// Mauvaise pratique
const uneLongueExpression = variable1 + variable2 + variable3 + variable4 + variable4 + variable5

// Bonne pratique
const bonnePratique = variable1 +
  variable2 +
  variable3 +
  ...
  variable5

// Mauvaise pratique
function uneFonctionAvecBeaucoupDeParams(params1, param2, params3, params4, params5, params6) {
  // content
}

// Bonne pratique
function uneFonctionAvecBeaucoupDeParams(params1,
  param2,
  params3,
  params4,
  params5,
  params6
) {
  // content
}
```

```html
<!--mauvaise pratique-->
<my-element attr1="value1" attr2="value2" attr3="value3" attr4="value4" attr5="value5" attr6="value6">
  Content
</my-element>

<!--bonne pratique-->
<my-element attr1="value1"
  attr2="value2"
  attr3="value3"
  attr4="value4"
  attr5="value5"
  attr6="value6"
 >
  Content
</my-element>

<!--bonne pratique car ne dépasse pas 80 caractères-->
<my-element attr1="value1" attr2="value2">
  Content
</my-element>
```
## Commentaire
Soyez généreux avec les commentaires. Il est utile de laisser des informations qui seront lues ultérieurement par des personnes (éventuellement vous-même) qui auront besoin de comprendre ce que vous avez fait. Les commentaires doivent être bien écrits et clairs, tout comme le code qu'ils annotent.

Il est important que les commentaires soient tenus à jour. Des commentaires erronés peuvent rendre les programmes encore plus difficiles à lire et à comprendre.

Donnez du sens aux commentaires. Concentrez-vous sur ce qui n'est pas immédiatement visible. Ne faites pas perdre le temps du lecteur avec des choses comme:
```js
i = 0 // NOTE: i initialisé à 0
```
### Préfixes des commentaires:
Les commentaires doivent obligatoirement avoir un préfixes pour rapidement savoir son type et faciliter les recherches
* NOTE: un commentaire explicatif concernant la ligne ou le bloc de code auquel il est attaché
```js
const variable = Math.round(calcul(input)) // NOTE: on arrondi le résultat pour s'assurer que l'affichage ne soit pas trop long
```
* FIXME: un commentaire notant une tâche à faire **EN URGENCE** par la suite mais qui ne fait pas l'objet du ticket en cours. Concerne surtout les codes temporaires qui sont là pour faire marcher une autre fonctionnalité
```js
// let result = calcul(input)
let result = 0 // FIXME: enlever cette ligne quand calcul() sera correctement implémenté
utiliser(result)
```
* TODO: un commentaire pour noter une prochaine tâche à faire
```js
const myMethod = (input) => { // TODO: prendre aussi en compte le param input1
  // content
}
```

## Déclaration de variable
Utilisez les mot-clés *const* et *let* pour déclarer les variables:
* `let`: variable qui est redéfini au moins une fois dans son scope
* `const`: variable qui n'est jamais redéfini dans son scope
```js
let i = 0
i++

const name = getName()
return traiter(name)
```

Notez qu'un objet n'est pas redéfini si on redéfini uniquement un ou plusieurs de ses attributs, et donc on utilisera *const*:
```js
const myObj = { a: 'test', b: 'value' }
myObj.a = format(obj.a)
return JSON.stringify(myObj)
```
## Déclaration de fonction
Préférez les expressions lambda autant que possible pour déclarer des fonctions, car elles gardent le même contexte de *this* que le scope où elles sont définies

```js
// Mauvaise pratique
const myFunction = function (var1, var2) {}

// Bonne pratique
const myFunction = (var1, var2) => {}
```
## Nommage
Les noms doivent être formés à partir des 26 lettres majuscules et minuscules (A .. Z, a .. z), des 10 chiffres (0 .. 9) et de _ (barre inférieure). N'utilisez pas de $ (signe du dollar) ou de \ (backslash) dans les noms.

La plupart des variables et des fonctions doivent commencer par une lettre minuscule.

Les fonctions Constructor et les noms de Classe qui doivent être utilisées avec le préfixe `new` doivent commencer par une majuscule. JavaScript n'émet ni un avertissement à la compilation ni un avertissement à l'exécution si un `new` obligatoire est omis. De mauvaises choses peuvent se produire si new n'est pas utilisé, la convention de capitalisation est donc la seule défense dont nous disposons.

Les variables globales et les constantes de configuration, si il y en a, doivent être écrites en majuscules.

```js
// Mauvaise pratique
const StoredPerson = await findPerson(id)
const _innerFilter = {}
const ma_longue_variable = 0
class person = {}
const $v = new Validator()

// Bonne pratique
const storedPerson = await findPerson(id)
const innerFilter = {}
const maLongueVariable = 0
class Person = {}
const validator = new Validator()
import { PAGE_CONFIG } from 'config'
```

## Espaces vides
Les lignes vides améliorent la lisibilité en séparant les sections de code qui sont logiquement liées.

Les espaces vides doivent être utilisés dans les circonstances suivantes :

Un mot-clé suivi de `(` (parenthèse gauche) doit être séparé par un espace.
```js
while (true) {}
```

Il ne faut pas utiliser d'espace entre une valeur de fonction et sa `( ` (parenthèse gauche). Cela permet de distinguer les mots-clés des invocations de fonctions.
```js
methods: {
  test () {
    // définition de la fonction
  }
}

test() // appel de la fonction
```

Tous les opérateurs binaires, à l'exception de `.` (point), `(` (parenthèse gauche) et `[` (parenthèse gauche), doivent être séparés de leurs opérandes par un espace.

```js
// Mauvaise pratique
const result=a+b
const bool = a ? 0:1
const value = array [1]
const person = findPerson (id)

// Bonne pratique
const result = a + b
const bool = a ? 0 : 1
const value = array[1]
const person = findPerson(id)
```

Aucun espace ne doit séparer un opérateur unaire et son opérande, sauf lorsque l'opérateur est un mot tel que `typeof`.

Chaque `;` (point-virgule) dans la partie contrôle d'une instruction `for` doit être suivi d'un espace.
```js
// Mauvaise pratique
for(let i=0;i < 10; i++)

// Bonne pratique
for (let i = 0; i < 10; i++)
```

Un espace blanc doit suivre chaque `,` (virgule).
```js
// Mauvaise pratique
let a,b
const array = [1,2,3]

// Bonne pratique
let a, b
const array = [1, 2, 3]
```

## Opérateurs === et  !==
Il est presque toujours préférable d'utiliser les opérateurs `===` et `!==`.
Les opérateurs `==` et `!=` font de la coercition de type. En particulier, n'utilisez pas `==` pour comparer des valeurs *falsy*.

```js
const a = ''
const b = false

console.log(a === b) // false
console.log(a == b) // true
```
## Conversion en nombre et chaîne de caractères
Pour effectuer proprement la conversion de variable en nombre ou chaîne de caractères, utilisez les constructeurs par défaut de Javascript `Number` et `String`

```js
const nombre = 10
const lettre = "10"

// Mauvaise pratique
console.log(+lettre) // 10
console.log(nombre + '') // "10"

// Bonne pratique
console.log(Number(lettre)) // 10
console.log(String(nombre)) // 10
```

N'oubliez pas que la conversion en nombre peut produire `NaN` quand la valeur n'est pas convertible
```js
const pasUnNombre = "test"
console.log(Number(pasUnNombre)) // NaN
```

## Attributs d'objet
### Vérification de présence d'un attribut dans un objet
Utilisez la fonction native `hasOwnProperty` pour s'assurer qu'un attribut est présent dans un objet.

```js
const result = getResult(params)

// Mauvaise pratique
if (response in result) {
  console.log(result.response)
}

// Bonne pratique
if (result.hasOwnProperty('response')) {
  console.log(result.response)
}
```

### Accès à un attribut
Utilisez l'opérateur `[` (crochet) pour accéder à un attribut d'objet qui est soit un nombre ou une chaîne de caractère contenant un `-` (tiret)
Utilisez l'opérateur `.` pour tout les autres cas

```js
// Mauvaise pratique
const name = myObj['name']

// Bonne pratique
const name = myObj.name
const url = configs.URL
const item = items[0]
const config = myObj['initial-config']
const name = myObj[attributName]
```

## Valeur de retour
Le mot-clé `return` est requis dans une fonction avec une valeur de retour, et ceux même si la fonction n'a qu'une seule instruction

```js
// Mauvaise pratique
const doubler = (number) => number * 2

// Bonne pratique
const doubler = (number) => { return number * 2 }
```
## Boucles et itérateurs
### Boucles asynchrones
Evitez d'écrire du code asynchrone dans une boucle `for` classique
Quand les instructions d'une boucle contiennent du code asynchrone utilisez la syntaxe `for await…of`

```js
const array = [1,2,3]

// Mauvaise pratique
for (let i = 0; i < array.length; i++) {
  const item = array[i]
  await traiter(item)
}

// Bonne pratique
for await (const item of array) {
  await traiter(item)
}

```

### map() VS forEach()
* `map` est à utiliser pour itérer sur un tableau et générer un nouveau tableau dont les éléments sont en fonction de ceux de l'original
* la valeur de retour de `map` doit être utilisé
* `map` doit avoir un callback qui retourne une valeur
* `forEach` est à utiliser pour itérer sur un tableau et effectuer une action pour chacun de ses éléments
* `forEach` n'a pas de valeur de retour
* `forEach` doit avoir un callback sans valeur de retour
```js
const array = [1, 2, 3]
const formattedArray = array.map((elem) => {
  return `Chiffre: ${elem}`
})

array.forEach((elem, i) => {
  console.log({ elem, i })
})
```
### Passage par référence
Il est **important** de se souvenir que le callback de `map` et `forEach` ont un premier paramètre "elem" qui est passé par référence SI ET SEULEMENT SI "elem" est un objet. Toute modification sur "elem" est effectif dans le tableau original. Cela peut entrainer des comportements inattendus si ce comportement n'est pas pris en compte

```js
const array = [1, 2, 3]
array.forEach((elem, i) => {
  elem = elem * 2 // NOTE: n'a aucun effet sur array
})
console.log(array) // [1, 2, 3]

const array1 = [{a: 1}, {a: 2}, {a: 3}]
array1.forEach((elem) => {
  elem.a = elem.a * 2 // NOTE: modifie array1
})
console.log(array1) // [{a: 2}, {a: 4}, {a: 6}]
```

### Spread Operator: (...object)
Le `Spread Operator` (...object) est un opérateur qui permet de copier les propriétés d'un objet vers un autre.

C'est l'une des méthodes pour éviter le problème que peut causer le passage par référence de `forEach` et `map`

```js
const obj1 = { a: '1', b: '2' }
const obj2 = { c: '1', d: '2' }
const merged = { ...obj1, ...obj2 }
console.log(merged) // { a: '1', b: '2', c: '1', d: '2' }
```
Utilisez le `Spread Operator` dans les cas qui le requiert
```js
const obj1 = { a: '1', b: '2' }
const obj2 = obj1 // NOTE: copie par référence
obj2.a = '11' // NOTE: affecte aussi obj1
console.log({obj1, obj2}) // { obj1: {a: "11", b: "2"}, obj2: {a: "11", b: "2"} }

//---------------------------------

const obj1 = { a: '1', b: '2' }
const obj2 = { ...obj1 } // NOTE: copie des attributs uniquement
obj2.a = '11' // NOTE: affecte seulement obj2
console.log({obj1, obj2}) // { obj1: {a: "1", b: "2"}, obj2: {a: "11", b: "2"} }
```

## Linter
### Fin de ligne
Evitez de mettre des `;` (point virgule) en fin de ligne. De même, il n'est pas nécessaire de terminer des listes de paramètres ou d'éléments d'un tableau avec une `,` (virgule)

### Chaînes de caractères
Utilisez `"` (double quote) pour délimiter une chaîne de caractères.

Pour concaténer des chaines de caractères, utilisez **`** (left quote), notamment pour insérer la valeur de variables. Evitez d'utiliser **+** (plus) pour la concaténation
```js
// Mauvaise pratique
const name = 'Luc'
const greeting = "Bonjour " + name + " !"

// Bonne pratique
const name = "Luc"
const greeting = `Bonjour ${name} !`
```

## Syntaxe Vue
### Ordre des balises vue
Dans un fichier Vue quelconque, respectez l'ordre des balises comme suit:
* template
*  script
* style
### Ordre des déclarations dans setup()
Un ordre de déclarations préétabli et ordonné nous permettra de retrouver la partie de la logique qui nous intéresse rapidement lors de la consultation d'un fichier Vue.
Dans cette optique, voici l'ordre des déclarations à suivre:
* compositions: useAuthStore(), useQuasar(), etc
* références: ref() et reactive()
* computed
* watch
* methods
* hooks: onMount, onBeforeUnmount, etc
* return { variables, functions, icons }
### v-if VS v-show
`v-if`  est un « vrai » rendu conditionnel car il garantit que les écouteurs d’évènements et les composants enfants à l’intérieur de la structure conditionnelle sont correctement détruits et recréés lors des permutations.

`v-if`  est également  **paresseux**  : si la condition est fausse sur le rendu initial, il ne fera rien (la structure conditionnelle sera rendue quand la condition sera vraie pour la première fois).

En comparaison,  `v-show`  est beaucoup plus simple. L’élément est toujours rendu indépendamment de la condition initiale, avec juste une simple permutation basée sur du CSS.

D’une manière générale,  `v-if`  a des couts à la permutation plus élevés alors que  `v-show`  a des couts au rendu initial plus élevés. Donc préférez  `v-show`  si vous avez besoin de permuter quelque chose très souvent et préférez  `v-if`  si la condition ne change probablement pas à l’exécution.

