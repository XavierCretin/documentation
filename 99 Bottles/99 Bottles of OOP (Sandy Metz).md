# Rediscovering Simplicity

Introduction au choix de conception entre ces deux concepts: DRY (et, par extension, l'abstraction) versus la maintenabilité.

DRY:

**Avantages**: 
lorsque le comportement change, on doit modifier le code à un seul endroit. L'argument avancé par OOD est que cela permettra de faire des économies.

**Inconvénient**:
Lorsque le code devient trop abstrait, en séparant une classe en une multitude d'autres par exemple, il est moins évident de comprendre le cas inital.

Tout le sujet de ce livre porte sur la _recherche de la bonne abstraction_.

> Design decisions inevitably involve trade-offs. - Sandy Metz

> You should not reach for abstractions, but instead, you should resist them until they absolutely insist upon being created. - Sandy Metz

## Simplifying Code

Introduction aux quatre solutions différentes au problème des "99 bouteilles de bière". 

### Incomprehensibly Concise

Le code se trouve [ici](https://github.com/sandimetz/99bottles_js/blob/2.0-initial-variations-10/lib/bottles.js#L1-L32)

Premier exemple d'un code qui contient peu de ligne de code ("concis"), mais avec un manque de cohérence, des duplications et aucun nom de variables.
Il en ressort un sentiment de forte incompréhension.

Il est dur de juger de la qualité de code, car cela relève du jugement de chacun.

Afin de refléter le domaine, il doit répondre aux questions suivantes :

*** Questons métiers ***

1. Combien de variantes de versets existe-t-il ?
2. Quels versets se ressemblent le plus ? De quelle manière ?
3. Quels versets sont les plus différents, et de quelle manière ?
4. Quelle est la règle pour déterminer quel verset vient ensuite ?

Aucune réponse ne peut être donnée avec la 1ère solution.

Les questions suivantes vous donneront une idée du potentiel coût d'un bout de code.

*** Question coût/valeur ***

- Quelle **était** la difficulté pour l'écrire ?
- A quel point c'**est** difficile de comprendre ?  <-- cette question s'applique toujours et nous concerne tous
- Est ce que ce **sera** cher de changer ?

Le code est facile à comprendre lorsqu'il reflète clairement le problème qu'il résout, et expose donc ouvertement le domaine de ce problème. 
Si ce n'est pas le cas, le code a probablement été difficile à écrire et sera difficile à modifier. 

### Speculatively General

Le code se trouve [ici](https://github.com/sandimetz/99bottles_js/blob/2.0-initial-variations-20/lib/bottles.js#L3-L65)

Deuxième exemple d'un code, toujours aussi peu clair et inutilement complexe à comprendre dû à trop de niveaux d'indirection.
Il expose bien les concepts clés du domaine, on voit l'effort pour apporter des extensions futures, mais le coût n'en vaut pas la peine.
L'évaluation du côut fait suite à ce constat trilatéral: C'était dur à écrire, c'est difficile à comprendre, et ce sera cher à changer.

### Concretely Abstract

Le code se trouve [ici](https://github.com/sandimetz/99bottles_js/blob/2.0-initial-variations-30/lib/bottles.js#L3-L104) 

Troisième exemple d'un code. Il y a aucune duplication de code, beaucoup de mini méthodes. 
Néanmoins, toutes ces indirections dûe à ce code DRY, et, par extension, le surplus d'abstraction, rend difficile la compréhension du code, et coûteux le changement.
Autre point négatif, non lié à DRY, est le couplage avec le mot `biere`. Si le problème initial portait sur une autre boisson qu'une bière, certaines méthodes doivent être renommées. Par exemple, la méthode `beer()`, qui ne fait que retourner le mot 'bière', a un nom mal choisit car le nom est trop lié à son implémentation actuelle. Si on change l'implémentaiton, le nom doit changer aussi !
Un meilleur nom: **drink** (boisson)

!!! tip
    You should **name methods** not after what they do, but after what they mean, what they represent in the context of your domain. - Sandy Metz

Si vous demandiez à votre client ce qu'est la `biere` dans le contexte de la chanson "99 Bottles", il ne répondrait pas "La bière, c'est la bière", mais quelque chose comme "La bière, c'est ce qu'on boit" ou "La bière, c'est la boisson (i.e `drink`)". 
le mot `drink` est un niveau d'abstraction supérieur à celui de `biere`. Nommer la méthode à ce niveau d'abstraction légèrement supérieur permet d'isoler le code des changements dans les détails de l'implémentation.

### Shameless Green

Le code se trouve [ici](https://github.com/sandimetz/99bottles_js/blob/2.0-initial-variations-40/lib/bottles.js#L3-L46) 

Quatrième solution, et la bonne. 
Extraordinairement simple, on distingue bien les élements clés du domaine
Le constat trilatéral en terme de côut est très bon: c'était facile à écrire, facile à comprendre, et peu cher à faire évoluer, avec les minimes duplications.

Produire un code trop DRY, avec beaucoup de niveaux d'indirections, produit souvent un effet opposé à ce qu'on souhaite initialement, c'est à dire 
un code facile à comprendre et à changer.
Par ailleurs, casser un code en de multiples méthodes apporte des niveaux d'indirection supplémentaires. 
Augmenter les indirections rend le code plus difficile à suivre.

>One of the biggest challenges of design is knowing when to stop.  - Sandy Metz

## Judging Code

Section qui va tenter de répondre à la question: "Qu'est ce qui rend un code bon ?"

### Evaluating Code Based on Opinion

Citations de ce qu'est un code 'bon':

>I like my code to be elegant and efficient. - Bjarne Stroustrup, inventor of C++
>Clean code is … full of crisp abstractions … — Grady Booch, author of Object Oriented Analysis and Design with Applications
>Clean code was written by someone who cares. — Michael Feathers, author of Working Effectively with Legacy Code

Aucune de ces définitions n'aident à atteindre cet état de qualité.
Les attributs qu'ils utilisent pour décrire un bon code sont qualitatifs et non quantitatifs.

### Evaluating Code Based on Facts

Introduction aux différentes métriques, qui sont des mesures d'une certaine qualité du code.

#### Source Lines of Code (SLOC)

Métrique insuffisante pour prédire la qualité d'un code.
Il existe en effet un lien entre la taille d'une application et le cout de maintenance, mais cette métrique
ne prend pas en compte, par exemple, la façon dont le code est organisé.

#### Cyclomatic Complexity

Complexité cyclomatique
    : La complexité cyclomatique d'une méthode est le nombre de points de décision de la méthode (if, case, while, ...) + 1 (le chemin principal).

Métrique également insuffisante, dont l'objectif tend à voir si un code est difficile à tester ou à maintenir.
Elle n'évalue que les conditions.
On peut l'utiliser pour comparer du code. Si on a deux variants de la même méthode, on peut les choisir en fonction de leur complexité cyclomatique.
Un code avec une faible compléxité cyclomatique est meilleur.
On peut aussi l'utiliser pour limiter la complexité génarale, en déterminant unevaleure maximale à ne pas dépasser.

#### Assignments, Branches and Conditions (ABC) Metric

Contrairement à la compléxité cyclomatique, cette métrique prend en compte non seulement les conditions (le 'C' dans ABC) mais aussi le nombre de variable assignées (le 'A') et le nombre de branches (qui ne sont pas des if) mais des appels de méthodes (le 'B').

Des scores élevés suggèrent que le code sera difficile à tester et coûteux à maintenir. Si vous pensez que votre code est simple mais que le score ABC dit le contraire, vous devriez y réfléchir à deux fois.


#### Comparing Solutions

On voit clairement que les métriques pointent en faveur de la bonne solution.

## Summary

Le danger avec l'expérience est de croire que la compléxité est inévitable.

La solution choisit parmis les autres est une qui _priorise la compréhensibilité en faveur à la modifiabilité_.

Cela ne veut pass dire que DRY est mauvais, mais plutôt qu'il est moins coûteux de gérer une duplication temporaire que de récupérer des abstractions incorrectes.

> Infinitely experienced programmers do not write infinitely complex code; they write code that’s blindingly simple. - Sandy Metz

# Test Driving Shameless Green

On va identifier dans cette section les différents tests à créer pour parvenir à la bonne solution.

##  Understanding Testing

Introduction au TDD

## Writing the First Test

Le premier est toujours plus dur à écrire.
Trouvez un point de départ et lancez-vous, en sachant qu'au fur et à mesure que vous avancez, le brouillard se dissipera

Identification d'une API. Celle proposée permet aux autres de requeter un seul couplet, une série de couplets ou la chanson entière.
La premier test peut satisfaire l'une des trois proposition. 

Le premier test choisit est celui qui consiste à vérifier le premier verset de la chanson.

## Removing Duplication

Le premier test vérifiait le verset pour la valeur haute (=99), le prochain teste la valeur basse(=3).
Ensuite quand vient le refactoring pour satisfaire les deux premiers tests, deux choix sont confrontés: un qui suggère des conditions (code [ici](https://github.com/sandimetz/99bottles_js/blob/2.0-c2-tests-90/lib/bottles.js#L2-L17) )
et un autre une interpolation (code [ici](https://github.com/sandimetz/99bottles_js/blob/2.0-c2-tests-100/lib/bottles.js#L2-L9))
En précisant que les métriques (SLOC, Complexité cyclomatique et ABC) sont à considérer avec une 'pincée de sel', on voit bien qu'ils sont en faveur de l'interpolation, ce qui rentre en corrélation avec ce que nativement on aurait choisit comme solution.

## Tolerating Duplication

Les deux premièers versets sont uniques et les voyants TDD y sont rouges.
Encore deux solutions sont proposées. Une qui consiste à brutalement ajouter la condition (code [ici](https://github.com/sandimetz/99bottles_js/blob/2.0-c2-tests-120/lib/bottles.js#L2-L18)) et l'autre à ajouter la condition directement dans la string (code [ici](https://github.com/sandimetz/99bottles_js/blob/2.0-c2-tests-130/lib/bottles.js#L2-L10)).

Dans le mauvais exemple, il y a une nouvelle méthode 'pluralizaiton' qui va pluraliser la string 'bottle' selon une condition. 

```javascript
 verse() {
 // ...
 `${number-1} ${this.pluralize(number)} of beer `
 // ...
 }
 pluralize(number) {
 return `bottle${number-1 === 1 ? '' : 's'}`;
 }
```
Pb: Le fait que le mot "bottle" soit répété plusieurs fois dans la chanson indique qu'il existe un concept sous-jacent qui n'a pas encore été déterré. Dans le domaine de la chanson, "bottle/bottles" représente quelque chose d'important, et cette chose n'est pas la pluralisation. 
Le code comme 'pluralize' est écrite quand les devs poussent DRY à l'extrême.

> DRY is important but if applied too early, and with too much vigor, it can do more harm than good.- Sandy Metz
> It’s better to tolerate duplication than to anticipate the wrong abstraction.- Sandy Metz

Lorsque vous êtes confronté à une telle situation, posez-vous ces questions : 
- Le changement que j'envisage rend-il le code plus difficile à comprendre ?
- Quel est le coût futur de ne rien faire maintenant ?
=> Si cela n'augmente pas vos coûts, retardez les changements.

Plus court, c'est souvent mieux, mais ce n'est pas le cas ici. 
Si le deuxième à l'avantage d'avoir moins de ligne, il est moins compréhensible. 
La briéveté a été choisit pour de mauvaise raison: éviter les duplication à tout prix a complexifier la solution, en obscurcissant le concept qui sous-tend les "bottles". 

## Hewing to the Plan

Deux nouveaux tests sont rajoutés: un pour 0 et un pour 1. 
Chacun à rajouter dans un switch plutot que un if car on vérifie l'égalité sur une valeur, plus parlant pour les lecteurs: les conditions attendues dans un switch sont presque les mêmes.

## Exposing Responsibilities

Réflection sur les tests de l'autre partie de l'API: une série de couplet, et en particulier `verses(a, b)`.

## Choosing Names

Est ce que le dernier morceau d'API à tester, à savoir la méhode `song()` vaut le coup ?  

```javascript
song() {
    return this.verses(99, 0);
}
```

**verses(99, 0)** et **song** produisent le même résultat, mais du point de vue de l'appelant, cela requiert moins de connaissance d'appeler `song`. En effet, c'est une chose de savoir que vous voulez (**quoi**) toutes les paroles de la chanson "99 Bottles", mais c'en est une autre de savoir **comment** Bottles produit ces paroles.

La connaissance qu'un objet a d'un autre crée une **dépendance**.
Sans la méthode `song` , l'appelant aurait beaucoup de dépendances: le nom de la méthode `verses`, le nombre d'argument, que le premier arguments est le verset par où commencer, et le deuxième par où finir, que ça commence au verset 0 et finir par le verset 99.

Cela exigerait trop de connaissance de la part de l'appelant si on n'avait pas eu cette méthode `song`. 

## Revealing Intentions

La méthode `verses` est l'**implémentation**, tandis que `song` est l'**intention**.
Les appelants `song` ne se préoccupent de savoir comment les paroles sont récupérées. 

## Writing Cost-Eective Tests

Il est très pénible de maintenir les tests si trop couplés avec le code de production. il faut éviter de changer les tests à chaque fois que le code change.

> Therefore, the first step in learning the art of testing is to understand how to write tests that confirm what your code does without any knowledge of how your code does it

## Avoiding the Echo-Chamber

Comment écrire le test qui vérifie la sortie de `song` :

cas 1:

```js
expect(bottles.song()).toBe(bottles.verses(99, 0))
```

Le test ci-dessus est mauvais: il ne vérifie pas que `song` produit les paroles attendues, mais qu'il produit la même chose `verse`. 
Couplage trop fort: si la signature de `verse` change, ça casse la sortie de `song`.
Pire encore, l'implémentation de `verse` peut changer sans casser le test, et sans produire la bonne chanson.

cas 2: 

```js
downTo(99, 0).map(i => bottles.verse(i)).join('\n')
```

Le couplage est encore plus fort. On vérifie ici que la sortie de `song` est comme l'implémentation de `verse`

cas 3: 

Dans le test, mettre toute les paroles dans un string et s'en servir comme expected.
Pb: la duplication! Et super long! => mais c'est la meilleure des solutions (voir ci-après)

## Considering Options

La meilleure option pour le test de `song` est le 3ème car il n'a pas de dépendance et survit aux changements de `Bottles`.
En effet, malgrès la longueur et la duplication (et, par extension, le côté non DRY) il n'y a pas de logique additionnelle pour abstraire le code dans le test.

> Tests are not the place for abstractions—they are the place for concretions.

## Summary

La solution `Shameless Green` n'est ni intelligente ni extensible. Sa valeur réside dans le fait que le code est facile à comprendre et peu coûteux à écrire. Si rien ne change jamais, cette solution est certainement assez bonne

Dans le prochain chapitre, il y a aura d'autres fonctionnalités.


# Unearthing Concepts

De nouvelles règles vont être apportées, et la bonne solution du précédent chapitre va évolué.

## Listening to Change

Pas de réfactoring prématurés, attendre les nouvelles spécifications. 
Elles nous guident sur la façon dont le code doit évoluer.

Nouvelle exigence : les utilisateurs ont demandé que vous modifiiez le code 99 Bottles afin de produire "1 six-pack" à chaque endroit où il est actuellement indiqué "6 bouteilles".
Le changement implique l'ajout de 2 nouvelles conditions dans le switch existant: c'est inacceptable. On doit arrêter ça.

## Starting With the Open/Closed Principle

Introduction aux principes SOLID.

L'ajout des 2 conditions dans le code ne respecte pas le principe Open/Closed.

Open-close principle
:  Les objets doivent être ouverts pour l'extension et fermés pour la modification.

Le code est "Open" à un nouveau requirement quand on peut l'atteindre sans modifier le code existant (par exemple, des conditions dans un switch).

> The "open" principle says that you should not conflate the process of moving code around, of refactoring, with the act of adding new features.

Cela veut dire que le processus de rendre un code "Open" se fait en deux temps: 
_Tout d'abord_, il faut réorganiser le code existant de manière à ce que _ensuite_ on puisse implémenter la feature en ajoutant à peine du code.
```
            +-------------+                      +--------------+
            |             | Yes                  |   Make the   |
  --------->| is it open? |--------------------->|    change    |
  |         |             |              ^       +--------------+
  |         +------+------+              |
  |                | No                  |
  |                |                     |
  |                v                     |
  |        +---------------+             |
  |        |  Do you know  | Yes  +------------+
  |        |  how to make  |----->|  Make it   |
  |        |   it open?    |      |    open    |
  |        +---------------+      +------------+
  |                | No
  |                |
  |                v
  |        +-------+---------+
  |        | Remove the      |
  ---------| easiest to fix/ |
           | best understood |
           | code smell      |
           +-----------------+
```
Dans la phase de réorganisation pour rendre le code plus "Open" au requirement, il est souvent difficile de savoir par où commencer.
Un moyen est d'identifier, puis de retirer, les **code smell**.

## Recognizing Code Smells

Introduction aux code smells, parus pour la premiere fois dans [ce livre](https://martinfowler.com/books/refactoring.html).

## Identifying the Best Point of Attack

Le code courant doit être recfactoriser pour accueillir ce nouveau changement. 
Dans la liste des code smell évidents, celui qui saute aux yeux est la duplication.

/!\ Avant de commencer à réfactoriser, il est important de préciser qu'il n'y a pas de lien direct entre la suppression de la duplication et le fait de réussir à rendre le code ouvert au nouveau requirement. C'est pourtant là toute la beauté de cette technique. Vous n'avez pas besoin de savoir comment résoudre l'ensemble du problème à l'avance. Le plan est de procéder à un examen minutieux, un code smell à la fois, en ayant foi que le chemin vers le principe "Open/Closed" sera révélé.

## Refactoring Systematically

Refactoring
:   Refactoring is the process of changing a software system in such a way that it does not alter the external behavior of the code yet improves its internal
    structure.

> Safe refactoring relies upon tests running green - Sandy Metz

Pendant la refacto, les tests existants qui vérifient le comportement actuel avant d'entamer la refacto sont primordials.
> Tests are the wall at your back. Successful refactorings _lean_ on green. Therefore, you should never change tests during a refactoring

Pendant la refacto, il est important de ne pas sauter des étapes. Cela aidera à découvrir des abstractions. 
_Si l'abstraction est difficile à trouver, alors se concentrer sur l'élimination des code smell peut grandement aider_. 
Et celles-ci devraient être éliminées en suivant les règles d'aggrégation (`flocking` en anglais) (section ci-dessous)

## Following the Flocking Rules

Difficile de trouver des différences entre les variants des paroles.
Cet ensemble de règles va aider à les identifier.

*** Règles d'aggrégation ***
```
1) Select thing that are most alike
2) Find the smallest difference between them
3) Make the simplest that will remive that difference
  a) Parse the new code
  b) Parse and execute it
  c) Parse, execute and use its results
  d) Delete unused code
```

* Pour l'instant, ne changez qu'une ligne à la fois.
* Exécutez les tests après chaque changement.
* Si les tests échouent, annulez-les et faites un meilleur changement.

> The focus here is encapsulating the concept that varies, a theme of many design patterns. - Gang of Four

## Converging on Abstractions

### Focusing on Difference

La différence est plus utile car elle a plus de signification. L'élimination de la similitude a une certaine valeur, mais l'élimination de la différence en a davantage.

> Difference holds the key to understanding. If two concrete examples represent the same abstraction and they contain a difference, that difference must represent a smaller abstraction within the larger one. If you can name the difference, you’ve identified that smaller abstraction. - Sandy Metz

L'idée est de ne pas s'avancer vers une abstraction immédiate, mais de suivre méthodiquement cette série de règles. 
En s'y attachant, les abstractions arriveront toutes seules, et, à la fin, cela aboutira à une solution _découverte_ par la réfactorisation. 

Application de la 1ère règle sur le problème: identifier ce qui est le plus semblable.
Les 2 cas où number = 2 et number = default sont très similaires. La seule différence est dans un mot, dans un cas c'est "1 bottle" et dans l'autre c'est "bottles".


### Simplifying Hard Problems

Les 2 chaines (presque) similaires ont été identifiéeS. La prochaine étape est de les rendre identiques.

### Naming Concepts

Comment nommer l'abstraction derrière les similarités des versets 2 et 3- 99 ? 
Deux informations peuvent nous aider dans le choix du nom: 
 - Le nouveau requirement: on parle de 'pack' donc 'bottle' est un terme mal choisit pour le concept
 - En général, il faut choisir un nom qui a un niveau d'abstraction juste au-dessus que celui de l'objet

> The name you choose will be the name you use in conversations with your customers.

Autre aide si on a toujours du mal à trouver un bon nom à cause de manque d'exemples concrets: imaginer d'autres choses dans la même catégorie (ex: vin)
Pour le poème, le nom choisie est `container`: parlant, compréhensible, et sans ambiguité.

Remplacer toutes les occurences de `bottle/bottles` par `container` dans le verset ? Non, ce serait trop préssipité et pourrait intégrer des bugs.
Il vaut meux faire de minuscules changements puis de lancer les tests (=> 3ème règle)

### Refactoring Gradually

On crée la méthode mais on utilise pas l'agument pour pas casser ceux qui ne l'utilisent pas
```javascript
container(number) {
 return 'bottle';
}
```
puis cette version qui ne casse pas la première
```javascript
container(number) {
 if (number === 1) {
 return 'bottle';
 } else {
 return 'bottles';
 }
}
```
Le code devient [celui-ci](https://github.com/sandimetz/99bottles_js/blob/2.0-c3-flocking-80/lib/bottles.js#L14-L50)

Dans cette nouvelle version, on continue d'exécuter uniquement le code précédent mais on a ajouté une nouvelle branche à la condition: c'est un mini-exemple du principe 'Open/closed'. Vous pouvez considérer cette modification comme une ouverture de la méthode `container` à une nouvelle exigence - lui permettant de retourner occasionnellement le mot "bottle".

Finalement, on fusionne les 2 versets en un seul, et le code final devient [celui-ci](https://github.com/sandimetz/99bottles_js/blob/2.0-c3-flocking-140/lib/bottles.js#L14-L47)

## Summary

# Practicing Horizontal Refactoring

On continue d'appliquer les **Flocking Rules** introduites dans le chapitre précédent pour abstraire davantage le code.

Après avoir identifier les différence entre les versets, il se trouve que le verset 1 et default sont les plus similaires. 
Donc ils s'imposent naturellement comme de bons candidats.

## Replacing Difference With Sameness

Remplacement les différences ligne par ligne des versets 1 et default.
Pour la première et deuxième: substitution par `${number}` et par la méthode précédemment crée `container`.

##  Equivocating About Names

Pour la troisième ligne, la différence est que dans l'un on utilise le mot `it` et dans l'autre le mot `one`. 
Le challenge est, comme toujours, d'identifier le concept et de lui donner un bon nom. Ici, c'est particulièrement difficile car sont très génériques. 

> Names should neither be too general nor too specific

Exemples: `thing` est trop générique, tandis que  `itOrOne` est trop restreint.

Lorsque le nom parfait pour un concept est insaisissable, il existe trois stratégies pour aller de l'avant:
 - Allouer 10mn avec dico à la main pour trouver le meilleur nom
 - Choisir un nom temporaire, genre `foo` ou `namethis` pour ne pas être bloqué
 - Demander de l'aide aux autres

Finalement, le mot `pronoun` est choisit, bien que trop général, le contept est clair.

## Deriving Names From Responsibilities

On vient de voir comme il est difficile de trouver un bon nom même quand on connait le contexte. Ca l'est encore plus quand on ne le connait pas.

Etude des dernières phrases à rendre similaires, et en particulier la différence entre `"no more"` et `${number-1}`
Choisir `remainder` peut paraitre une bonne idée mais on voit que non car en se projettant plus loin le verset 0 commence par `No more` . Ainsi, dans le poème, à chaque fois que il y a 0 bouteille, on chante `no more`, capitalisé ou non.
Le nom choisit est `quantity`

> When creating an abstraction, first describe its responsibility _as you understand it at this moment_, then choose a name which reflects that responsibility. The effort you put into selecting good names right now pays off by making it easier to recognize perfect names later.

## Choosing Meaningful Defaults

Après pas mal de remaniements pour incorporer la méthode Quantity(), les 2 versets deviennent identiques. La solution [ici](https://github.com/sandimetz/99bottles_js/blob/2.0-c4-flocking-240/lib/bottles.js#L14-L34) 

## Seeking Stable Landing Points

Récapitulatif: 3 nouveaux concepts ont été identifiés:
`quantity`, `pronoun` et `container`.
Chaque méthode a une responsabilité. Identiques dans la forme, elles ont toutes le même argument. Ce n'est pas un hasard, c'est lié à la refacto!

> Rearranging code is like rock hopping down a stream (= sauter à cloche-pied). If you follow the rules of refactoring, you’ll quickly pass over the slippery places, and arrive at stable, consistent resting points.


## Obeying the Liskov Substitution Principle

Concentrons nous sur les 2 versets restants: 0 et default.

1ère différence: `No more` et `no more`. Le premier est capitalisé.
Ils représentent le même concept ("no more" doit être chanté lorsque le nombre de bouteilles est égal à 0). Ces mots sont une chose, et savoir s'ils doivent être mis en majuscules en est une autre. Peut-être que la connaissance des mots appartient à un endroit, et la connaissance des exigences de la capitalisation se trouve ailleurs. La nouvelle méthode va donc décorer quantity comme ceci: `capitalize(this.quantity(number))}` 

Il y a un problème avec la valeur par défaut de `quantity(number)` qui retourne un entier, et pas une chaine. Donc si on lance les tests, on a une exception.
Pour corriger, on peut transformer en  `capitalize(this.quantity(number).toString())}` mais là ça ne va pas.
En effet, l'appelant a trop de connaissance pour l'envoie du message.
> Every piece of knowledge is a dependency

L'idée de réduire le nombre de dépendances imposées aux émetteurs de messages en exigeant que les récepteurs renvoient des objets fiables est une généralisation du **principe de substitution de Liskov**.

Liskov: 
    "les sous-types doivent être substituables à leurs supertypes".

Liskov, en termes simples, exige que les objets soient ce qu'ils promettent d'être. 

> Liskov prohibits you from doing anything that would force the sender of a message to test the returned result in order to know how to behave. - Sandi Metz


Violation du principe LSP avec la méthode `quantite`.
Liskov vous interdit de faire quoi que ce soit qui obligerait l'expéditeur d'un message à tester le résultat renvoyé afin de savoir comment se comporter. Les récepteurs ont un contrat avec les expéditeurs. Dans la méthode  `quantité`, l'un des retours respectait le contrat "capitalisable" et l'autre non. Une incohérence comme celle-ci oblige très souvent l'expéditeur à mettre en œuvre une condition pour identifier et corriger le retour errant.
Il faut que quantité respecte le contrat en fournissant un retour cohérent. Au lieu de forcer la méthode `verse` à résoudre ce problème, la quantité devrait retourner un objet digne de confiance, 'capitalisable'


## Taking Bigger Steps

Identification de deux phrases qui sont complêtement différentes: 
```
'Go to the store and buy some more, '
```
et 
``` 
'Take ${this.pronoun(number)} down and pass it around, '.
```
On doit nommer le concept, créer une méthode, puis remplacer cette différence par un message envoyé.
La première étape consiste donc à nommer la catégorie dans laquelle ces deux phrases sont des exemples concrets.
Cette partie de la chanson traite de ce qui se passe en fonction du nombre actuel de bières. S'il y a des bières, vous en buvez une. Sinon, vous allez faire des courses. Ces lignes décrivent l'**action** à entreprendre, c'est donc un bon nom pour ce concept.

L'introduction de la méthode `action` donne ce [résultat-ci](https://github.com/sandimetz/99bottles_js/blob/2.0-c4-flocking-350/lib/bottles.js#L14-L41).

## Discovering Deeper Abstractions

## Depending on Abstractions

Voici ce que donne la classe `Bottle` avec les [versets unifiés](https://github.com/sandimetz/99bottles_js/blob/2.0-c4-flocking-440/lib/bottles.js#L3-L70).

## Summary

Le code est meilleur: Les concepts de conteneur, pronom, quantité, action et successeur étaient invisibles dans `Shameless Green`, mais sont à la fois révélés et isolés dans ce nouveau code.

# Separating Responsibilities

## Selecting the Target Code Smell

Le code existant n'est toujours pas prêt pour accueillir le nouveau requirement '6 pack' (non respect du open close principle)

> The truth about refactoring is that it sometimes makes things worse

Il se peut qu'on ne sache pas encore, après refacto faite, quels changements adoptés pour acceuillir le nouveau requirement.
Un choix s'offre à nous, soit on revert la refacto et on prend un nouvelle voie, soit on modifie le code actuel.

La refacto qu'on vient de faire a amélioré le code, cela suggère qu'on peut aller de l'avant.

### Identifying Patterns in Code

### Spotting Common Qualities

Quand on regarde le code, ce qui saute aux yeux est la forme commune des méthodes.
En particulier les méthodes prennent le même argument 'number'. Avoir plusieurs méthodes qui prennent le même argument est un code smell. Il est important, cependant, de reconnaître qu'ici le terme "même" signifie même concept, et non pas même nom. 
Dans un monde idéal, chaque concept différent aurait son propre nom, unique et précis, et il n'y aurait aucune ambiguïté.
L'argument 'number' a deux sens: celui du  numéro de verset et celui du nombre de bouteille.

**POO: Insister sur les messages: exemple avec un anti pattern pour la POO**

Ci dessous un exemple de anti-pattern pour la POO:
```js
container(number) {
 if (number === 1) {
 return 'bottle';
 } else {
 return 'bottles';
 }
}
```
Anti pattern: Les 5 méthodes presque similaires prennent un argument, l'examine, et _lui fournisse un comportement_.

Crtique sur le code: `container` a une dépendance vers `number`. Non seulement il connait `number`, mais en plus il doit comprendre à quoi correspond spécifiquement certaines de ses valeurs.  Si l'une de ces deux depandances change, la méthode du `container` pourrait être obligée de changer à son tour.


**POO: Le potentiel code smell des conditions**
> "As an OO practitioner, when you see a conditional, the hairs on your neck should stand up. Its very presence ought to offend your sensibilities. You should feel entitled to send messages to objects, and look for a way to write code that allows you to do so. The above pattern means that objects are missing, and suggests that subsequent refactorings are needed to reveal them. Be on the lookout for this code shape, as it implies that there’s more to be done." - Sandi Metz.

Cela ne veut pas dire que vous n'aurez jamais de conditionnel dans une application orientée objet. Il y a une place pour les conditionnels dans l'OO. Les applications OO gérables sont constituées de pools de petits objets qui collaborent pour accomplir des tâches. Les collaborateurs doivent être rassemblés dans des combinaisons utiles, et assembler ces combinaisons nécessite de savoir quels objets sont appropriés. Un objet quelconque, quelque part, doit choisir les objets à créer, et cela implique souvent un conditionnel.

Cependant, il y a une grande différence entre une condition qui sélectionne l'objet correct et une autre qui fournit un comportement. La première est acceptable et généralement inévitable. La seconde suggère qu'il vous manque des objets dans votre domaine

> Code is striving (en fr "vise") for ignorance, and preserving ignorance requires minimizing dependencies. - Sandy Metz

Code smell detecté: `Primitive Obsession`
> Primitive Obsession is when you use one of these data types to represent a concept in your domain. Obsessing on a primitive results in code that passes built-in types around, and supplies behavior for them  - Sandy Metz

Le remède pour Primitive Obsession est de créer une classe à la place d'une primitive en utilisant la recette de `Extract class`.

**Le pouvoir de la POO**

La nouvelle classe: `BottleNumber` à créer n'est pas une "chose" comme une Bottle, mais plutôt une "idée" car n'existe pas dans le monde physique.
Un peu comme dans une application de gesiton d'événement où`Purchase` ou `Dicount` ou `Refund` seraient qualitifées de virtuelle alors que `Ticket` ou `Acheteur` seraient bien réels. Ces objets interagissent de de nombreuses façons : les acheteurs achètent des billets, peut-être à prix réduit, et peuvent ensuite changer d'avis et renvoyer les billets pour être remboursés.

Où, dans une telle application, devrait se trouver la logique de gestion des achats, des remises et des remboursements ? Vous pourriez tout regrouper dans Buyer et Ticket, mais la puissance de l'OO est qu'elle vous permet de créer un monde virtuel dans lequel `Purchase`, `Discount` et `Refund` sont tout aussi réels. L'incorporation de ces concepts dans des classes distinctes sépare les responsabilités et rend l'application globale plus facile à comprendre, à tester et à modifier.
Les programmeurs OO expérimentés créent habilement des mondes virtuels dans lesquels les idées sont aussi réelles que les objets physiques. Si vous n'êtes pas encore à l'aise avec cette méthode, commencez dès aujourd'hui à considérer la classe que vous êtes sur le point d'extraire non pas comme une bouteille physique, mais comme un nombre symbolique auquel on a ajouté un comportement de bouteille.

**Nommer les méthodes versus nommer les classes**

La règle "name things at one higher level of abstraction" s'applique davantage aux méthodes qu'aux classes. Il serait spéculatif d'appeler cette nouvelle classe `ContainerNumber`. La règle de nommage peut donc être modifiée : alors que vous devez continuer à nommer les méthodes en fonction de _leur signification_, les classes peuvent être nommées en fonction de _ce qu'elles sont_.

**Résultat de l'exemple après refacto en appliquant _Extract Class_**

```js
class Bottles {
  // ...
  quantity(number) {
   return new BottleNumber(number).quantity();
 }
 ```

Après extraction du code de `Bottles` vers `BottleNumber` , le premier est dépourvu de conditions, mais elles n'ont pas disparues.

**Immutabilité**

Les objets immuables sont ceux qui ne changent pas. Dans un monde fonctionnel, lorsqu'une tasse de café est vide, elle n'est pas remplie à nouveau, elle est remplacée par une nouvelle tasse de café déjà pleine. Comme toute chose dans la vie, cela comporte des coûts et des avantages. L'avantage de ne jamais faire muter les objets est que ces objets sont très stables et prévisibles. Le coût est que la création d'un nouvel objet à chaque fois qu'il doit être modifié peut affecter les performances.

_Avantages_:

- Comme on change pas l'objet d'origine, le comportement est très prédictible. Et de facto, très facile à tester. Les objets qui changent ont besoin de tests pour le comportement affecté. Le changement peut être causé par un objet collaborateur, ou déclenché par un événement distant, de sorte que les tests peuvent nécessiter des collaborateurs supplémentaires.
- Une autre vertu clé des objets immuables est qu'ils sont thread safe. Certains des bogues les plus pernicieux dans les systèmes multithreads impliquent la modification par inadvertance de l'état partagé par différents threads. Ces bogues sont souvent liés à la synchronisation de l'exécution des threads, et sont donc notoirement difficiles à reproduire, ainsi que coûteux et frustrants à déboguer. Cette catégorie de problèmes est entièrement évitée par les objets immuables.

Par conséquent, il existe de nombreuses bonnes raisons de préférer les objets qui ne mutent pas. Vous n'êtes retenu de les créer que par l'habitude de la mutabilité, et l'hypothèse (souvent incontestée) que l'instanciation de nouveaux objets sera inacceptablement plus coûteuse que la réutilisation des objets existants. 

Si c'était grauit, on utiliserait que des objets immuables. Mais le co$ut pour créer les nouveaux objets peut etre cher.

**Cache: solution à l'immutabilité ? Pas forcément...**

Un cache est une copie locale d'un élément stocké ailleurs. Sauvegarder une copie locale des résultats d'une opération coûteuse, ou la mettre en cache, permet d'augmenter la vitesse de votre application et de réduire les coûts. La mise en cache est facile, mais savoir quand la mise à jour est nécessaire ne l'est pas. Parfois, la gestion du cache est si compliquée qu'elle n'en vaut pas la peine.

> The best programming strategy is to write the simplest code possible and measure its performance once you’re done. If the whole is not acceptably fast, profile the performance, and speed up the slowest parts. Increasing speed may require caching, but many problems can be fixed by substituting more efficient code in specific, narrow places. - Sandy Metz

Votre objectif est d'optimiser la facilité de compréhension tout en maintenant des performances suffisamment rapides. Ne sacrifiez pas la lisibilité au profit de données de performance solides. La première solution à tout problème devrait éviter la mise en cache, utiliser des objets immuables et _considérer la création d'objets comme gratuite_.

**Retour sur l'exemple avec ce qu'on sait du cache**

Le code existant crée 900 instances de `BottleNumber` :o.
On peut cacher, avec une invalidation simple (le plus fastidieux quand il s'agit de cacher) en créant un `BottleNumber` pour chaque verse. 

```js
1 verse(number) {
2 const bottleNumber = new BottleNumber(number);
5
6 return (
7 `${capitalize(bottleNumber.quantity())} ` +
8 `${bottleNumber.container()} ` 
8 // ...
```

# Chapter 6 : Atteindre l'ouverture

**Bilan du code actuel**

Le code n'est toujours pas ouvert au nouveau requirement.
Néanmoins, la nouvelle classe `BottleNumber` contient des méthodes qui isolent les choses nécessaires au changement. 
On pourrait s'arrêter là et ajouter des conditions dans les méthodes pour accueillir le nouveau requirement.

!!! success
    Cette isolation croissante des concepts qui doivent varier est une indication que le code évolue dans la bonne direction.

**Détection d'un code smell: Data Clump**

Data Clump
: A data clump is defined as the situation in which several (three or more) data fields routinely occur together.

Dans le code, `container` et `quantity` viennent toujours en pair.

Remède: d'habitude on applique `Extract class` mais ici on peut simplement extraire les deux dans une méthode.

Après refacto:
```js
class BottleNumber {
2 // ...
3 toString() {
4 return `${this.quantity()} ${this.container()}`;
5 }
```

**Identificaiton du code smell *"Switch Statement"*: trop de conditions dans la classe `BottleNumber`**

L'extraction de `BottleNumber` a certainement supprimé les conditionnels de Bottles, mais ils n'ont pas disparu : ils ont simplement été déplacés vers la nouvelle classe extraite. Bien qu'elles soient légèrement améliorées dans la mesure où les méthodes utilisent maintenant la propriété `number` plutôt que de prendre un argument, elles contiennent toutes (à l'exception de toString) encore des conditionnelles.

2 Remèdes au code smell *"Switch Statement"*:

- *Conditional with State/Strategy*
Supprime les conditionnels en dispersant leurs branches dans de nouveaux objets plus petits, dont l'un est ensuite sélectionné et rebranché au moment de l'exécution. Cette recette donne lieu à un arrangement de code connu sous le nom de *composition*.

- *Replace Conditional with Polymorphism*
La recette supprime les conditionnels en créant une classe pour contenir les valeurs par défaut des conditionnels (les branches fausses), et en ajoutant des sous-classes pour chaque spécialisation (les branches vraies des différentes conditions). Elle choisit ensuite l'un de ces nouveaux objets à réintégrer au moment de l'exécution. Cette recette résout le problème des conditionnelles en utilisant l'héritage.

Les deux sont très similaires: le premier utilise la composition et l'autre l'héritage.

**Retour au code: application de *Replace Conditional with Polymorphism***

Il y a des nombres spéciaux, comme 0 ou 1. La logique spécifique pour ces deux cas méritent d'être isolé dans leur propre classe.
2 nouvelles classes: BottleNumber0 et BottleNumber1.
```js
// ex avec BottleNumber0
class BottleNumber0 extends BottleNumber {
2 quantity() {
3   return 'no more';
4 } 
5}
```
```js
const bottleNumber =  new (number === 0 ? BottleNumber0 : BottleNumber)(number);
const succ = bottleNumber.successor();
const nextBottleNumber =  new (succ === 0 ? BottleNumber0 : BottleNumber)(number);
```
!!! danger
    Présence de duplication de la condition pour choisir la bonne instance

**La *Factory* pour n'en choisir qu'un**

Lorsque plusieurs classes jouent un rôle commun, un code, quelque part, doit savoir comment choisir la bonne classe qui effectue le rôle approprié. Ce choix implique très souvent un conditionnel, qui doit exister _à un et un seul_ endroit. On dit d'un code de ce type qu'il "fabrique" une instance du bon type d'objet, c'est pourquoi on l'appelle communément une *Factory*.

La *Factory* a pour seule responsabilité de créer les objets qui ont le bon rôle. Son but est d'isoler les noms des classes concrêtes et de cacher la logique derrière pour choisir la bonne class.

**Retour au code: application de *Factory***

*Factory Method*
```js
1 class Bottles {
2   // ...
3   bottleNumberFor(number) {
4   if (number === 0) {
5     return new BottleNumber0(number);
6   } else {
7     return new BottleNumber(number);
8   }
9 }
```

**La force du polymorphisme**

`BottleNumber` et `BottleNumber0` jouent tous deux le _rôle_ de numéro de bouteille.
 Ils répondent aux mêmes messages et se conforment à la même API, mais iplémentent `quantity` de manière complètement différente.

Ces classes sont substituables les unes aux autres. Lorsque vous invoquez la fabrique pour obtenir un numéro de bouteille, vous n'avez pas besoin de connaître la classe de l'objet renvoyé. Vous faites simplement confiance à cet objet pour agir comme un numéro de bouteille et répondre aux messages que vous prévoyez d'envoyer.

**Recette finale combinant *Replace Conditional with Polymorphism* et *Factory***

```
1. Create a subclass to stand in for the value upon which you switch.
  a. Copy one method that switches on that value into the subclass.
  b. In the subclass, remove everything but the true branch of the conditional.
    i. At this point, create a factory if it does not yet exist, and
    ii. Add this subclass to the factory if not yet included.

  c. In the superclass, remove everything but the false branch of the conditional.
  d. Repeat steps a-c until all methods that switch on the value are dispersed.

2. Iterate until a subclass exists for every different value upon which you switch.
```

**Liskov toujours violé**

Quand on regarde le code suivant, `successor` échoue à garder la promesse d'_agir_ comme un numéro de bouteille. A la place, elle retourne pour `verse` un résultat inattendu: un entier au lieu d'un numéro de bouteille.

```js
// ... dans la classe Bottles
const nextBottleNumber = this.bottleNumberFor(bottleNumber.successor());
// ... dans la classe BottleNumber
static for(number) {
 if (number instanceof BottleNumber) { // <= check du type: violation apparente de liskov
  return number;
 }
```

Après modification:

```js
 verse(number) {
 const bottleNumber = BottleNumber.For(number);

 return (
 // ...
 `${bottleNumber.action()}, ` +
 `${bottleNumber.successor()}  of beer on the wall.\n` 
 // ...

 class BottleNumber {
  static for(number) {
  // ...
  }
  successor() {
  return BottleNumber.for(this.number - 1);
  }
 }
}
```

**Le mot de la fin de Liskov**

Les objets qui, parfois, ne répondent pas à un message que vous prévoyez d'envoyer, ou qui renvoient occasionnellement quelque chose que vous n'attendez pas, vous obligent à adopter un style de programmation paranoïaque. Ces objets indignes de confiance exigent des expéditeurs de messages qu'ils en sachent trop. 

Lorsque votre application contient du code qui a besoin de connaître les données internes d'autres objets afin d'interagir correctement avec eux (comme l'a fait `successor` ci-dessus), les modifications apportées à ces autres objets peuvent casser votre code. Si vous devez vérifier le type d'un objet afin de savoir quel message envoyer, vous êtes contraint d'utiliser un conditionnel qui liste chaque classe concrète avec laquelle vous êtes prêt à collaborer. Ce faisant, vous vous condamnez à modifier la conditionnelle lorsque vous ajoutez une nouvelle classe. Vérifier si un objet répond à un message plutôt que de vérifier le type de cet objet peut réduire la taille de cette condition, mais ne résout pas le problème.

Tous les cas ci-dessus sont des symptômes d'une incapacité à faire confiance aux autres objets, et les échecs de confiance sont, au moins selon l'interprétation généreuse actuelle du principe, des violations de *Liskov*. Les objets ont fait des promesses qu'ils n'ont pas tenues. Dans tous les cas, la cause sous-jacente est une utilisation insuffisante du polymorphisme.

**Retour à l'exemple: le nouveau requirement 6 pack enfin implémenté!**

Ajout de la nouvelle classe qui fait 9 lignes et l'ajouter dans la factory

```js
1 class BottleNumber6 extends BottleNumber {
2 quantity() { 
3 return '1';
4 }
5
6 container() {
7 return 'six-pack';
8 }
```

>💡 "Make the change easy (warning: this may be hard), then make the easy change" — Kent Beck

# Chapitre 7

**Etude approfondie de la factory**

La fabrique `BottleNumber.for` contenait une condition codée en dur pour choisir la bonne classe, et cette condition devait être mise à jour pour inclure le nouveau nom de classe.

**LA POO repose sur le polymorphisme**

> "**Polymorphism** results in multiple classes that play a common role. The power of polymorphism is that these role-playing objects are interchangeable from the message sender’s point of view." - Sandy Metz

Les expéditeurs de message savent ce que font leurs collaborateurs, mais refusent d'être conscients de la manière dont ils le font. 

Les expéditeurs de messages ne sont pas autorisés à connaître les noms des classes des différentes classes concrètes, ni à connaître la logique nécessaire pour choisir entre elles. Ces connaissances incombent plutôt les **factory**. 

**Réflection sur la factory : étude des différents façon de choisir le bon objet**

La version 'classique' reposant sur une simple condition:

```javascript
class BottleNumber {
  static for(number) {
  let bottleNumberClass;
  switch (number) {
    case 0:
    bottleNumberClass = BottleNumber0;
    break;
    case 1:
    bottleNumberClass = BottleNumber1;
  break;
  case 6:
  bottleNumberClass = BottleNumber6;
  break;
  default:
  bottleNumberClass = BottleNumber;
  break;
 }

 return new bottleNumberClass(number);
 }
```

Défaut: Posséder la logique de choix a du sens lorsqu'elle est simple et stable, comme dans l'exemple actuel. Mais il est facile d'imaginer des situations où la logique de choix est beaucoup plus compliquée. La logique nécessaire pour sélectionner la bonne classe peut être longue, complexe et plus étroitement liée à la classe choisie qu'à la fabrique elle-même. 
Si la logique de choix change en même temps que le code qui vit dans la classe choisie, alors la logique de choix appartient à cette classe, et non à la fabrique. Dans ce scénario, chaque objet pouvant être choisi implémente sa propre méthode pour déterminer s'il doit être choisi. 
La fabrique itère alors sur les objets possibles et leur demande de prendre la décision.

Voici un tel exemple:

```javascript
class BottleNumber {
  static for(number) {
  const bottleNumberClass = [
  BottleNumber6,
  BottleNumber1,
  BottleNumber0,
  BottleNumber,
  ].find(candidate => candidate.canHandle(number));
 
 return new bottleNumberClass(number);
 }

 static canHandle(number) {
 return true;
 }
 // ...
 }

 class BottleNumber0 extends BottleNumber {
 static canHandle(number) {
 return number === 0;
 }
 // ...
 }

 class BottleNumber1 extends BottleNumber {
 static canHandle(number) {
 return number === 1;
 }
 // ...
 }

 class BottleNumber6 extends BottleNumber {
 static canHandle(number) {
 return number === 6;
 }

```

Chaque classe implémente la méthode !`canHandle()`.
Cette fabrique disperse la logique du choix dans les objets choisis. Dans cet exemple, cette logique est si simple que cette technique est excessive, mais dans certaines situations, le choix implique beaucoup de code, et ce code changera en même temps que la classe choisie. Ce sont les cas où cette technique vous fait économiser de l'argent.

Défauts de cette méthode: 
- Lors d'un ajout d'un, la factory doit le rajouter dans la liste
- Plusieurs objets peuvent retourner true à la méthode !`canHandle()`, et la fabrique retourner le 1er qui satisfait la condition.
- Si un objet apparait après la classe de basse dans l'itération des objets, alors il ne sera jamais retourné.

L'exemple ci-dessus montre la manière la plus simple de disperser la logique de choix. C'est peut-être tout ce dont vous avez besoin. Si vous souhaitez conserver l'ordre de la liste, ou si vous devez gérer le cas de plusieurs candidats souhaitant se porter volontaires, vous devrez écrire un peu plus de code.

Enfin, la dernière pour ajouter un candidat à la factory sans la modifier serait de laisser les classes qui souhaitent figurer sur la liste demander explicitement à l'usine de les y inscrire. 

```javascript
 class BottleNumber {
  static for(number) {
  const bottleNumberClass = BottleNumber.registry.find(
  candidate => candidate.canHandle(number)
  );
 
  return new bottleNumberClass(number);
  }
 
 static register(candidate) {
 BottleNumber.registry.unshift(candidate);
 }
 // ...
 }
BottleNumber.registry = [BottleNumber];

 class BottleNumber0 extends BottleNumber {
 // ...
 }

 BottleNumber.register(BottleNumber0);

 class BottleNumber1 extends BottleNumber {
 // ...
}
```

!!!! Cela suppose que la relation entre `BottleNumber` et `BottleNumberN` doit être une relation d'héritage afin d'accéder à la méthode `register()`

## Inverting Dependencies - Thèmes de l'injection de dépendance et de l'inversion de dépendance

Avec le code suivant, on a une dépendance de `Bottle` vers `BottleVerse` car il connait le nom de cette classe concrête.

```javascript
class Bottles {
  song() {
  return this.verses(99, 0);
  }
 
  verses(starting, ending) {
  return downTo(starting, ending)
  .map(i => this.verse(i))
  .join('\n');
 }

 verse(number) {
 return new BottleVerse(number).lyrics();
}
}
```

_Problème_: 
`Bottle` ne peut pas collaborer avec une autre classe que `BottleVerse` et ne peux obtenir de lyric d'une autre classe.

_Solution_: 
Réduire le couplage entre ces deux classes via l'injection de dépendance, de façon à ce que `Bottle` puisse collaborer avec n'importe quelle autre classe qui joue le *rôle* de lyric provider.

Voici le processus utilisé pour créer le nouveau rôle :
1. Identifiez le code que vous voulez faire varier.
2. Nommez le concept sous-jacent.
3. Extraire le code identifié dans sa propre classe.
4. Injectez ce nouvel objet de jeu de rôle dans l'objet dont il a été extrait.
5. Transférer les messages de la classe d'origine vers l'objet injecté.

En illustration, cela donne.

![](images/extract_and_inject_dependency.PNG)

Ce processus peut être résumé en quelques mots : *Isolez le comportement que vous voulez faire varier*.

> One of the most fundamental concepts in OO is to isolate the behavior you want to vary. - Sandy Metz

Lorsqu'une modification est nécessaire dans une petite partie de votre code, l'extraction et l'injection d'un objet qui joue un rôle ouvre la possibilité de créer et d'injecter d'autres objets qui jouent le même rôle. Le point d'injection devient un point de jonction à travers lequel les objets interagissent d'une manière faiblement couplée. Ces liens permettent aux applications de s'étendre et de prendre en charge de nouveaux comportements sans avoir à modifier le code existant. 

Le nom technique de ce qui s'est passé lors du remaniement précédent est l'inversion des dépendances. Le principe d'inversion des dépendances (DIP) -le D dans SOLID, peut prêter à confusion, car son nom même suppose que vous le comprenez déjà.

!!!! hint La clé pour comprendre le principe de DIP est de reconnaître que votre code doit dépendre d'abstractions

DIP dit que vous devriez inverser ces dépendances et dépendre des abstractions à la place.

Là où `Bottles` avait une dépendance sur la classe concrête `BottleVerse`, il a maintenant une dépendance sur l'abstraction d'un model de vers. Ainsi, la dépendance d'origine a été inversée.

Cette définition: "dépend des abstractions, pas des concrétions" distille l'essence de la définition DIP originale plus verbeuse du numéro de mai 1996 de C++ Report, qui l'expliquait comme suit :

1. High-level modules should not depend upon low-level modules. Both should depend upon abstractions.
2. Abstractions should not depend upon details. Details should depend upon abstractions

Notez que le mot "module" dans la définition ci-dessus ne fait pas référence à une fonctionnalité spécifique du langage. Vous pouvez substituer les mots "classes" ou "objets" aux "modules".

`Bottles` est donc le module de plus haut niveau dans le code. Une autre façon de voir les choses est que `Bottles` est la classe la plus externe. L'API publique entière est actuellement définie dans `Bottles`. Toutes les autres classes ont été créées en extrayant le comportement du module de haut niveau  `Bottles`. Ces classes extraites représentent des modules de niveau inférieur.

Les noms de classe sont des concrétisations. Lorsque `Bottles` contenait une référence codée en dur à `BottleVerse`, il dépendait directement de cette concrétion. Ainsi, un module de niveau supérieur avait une dépendance concrète sur un module de niveau inférieur. Il dépendait d'un détail plutôt que d'une abstraction.

En termes de code actuel, la définition officielle du principe d'inversion de dépendance peut être reformulée comme suit : 
1. Les modules de haut niveau comme `Bottles` ne devraient pas dépendre de modules de plus bas niveau comme `BottleVerse`. Chacun devrait dépendre d'abstractions.
2. `Bottles` ne doit pas dépendre de détails concrets comme le nom de la classe `BottleVerse`. `Bottles` devrait plutôt dépendre d'un objet qui génère polymorphiquement des versets de chansons.

Alternativement, en utilisant la forme courte de la définition DIP du chapitre 3, vous pourriez dire : 
* `Bottles` devrait dépendre de l'abstraction verseTemplate plutôt que de la concrétion `BottleVerse`.

Isoler des variantes nécessite souvent d'inverser les dépendances, et une excellente technique pour inverser les dépendances est de les _injecter_. 

Cette section a isolé la variante `BottleVerse`, puis a inversé la dépendance en injectant `BottleVerse` en tant que acteur de modèle de vers. 
`Bottles` dépend du rôle de modèle de verset. `BottleVerse` joue ce rôle. 
Puisque `Bottles` dépend maintenant d'un rôle abstrait plutôt que d'une classe concrète, vous pouvez créer et injecter joueurs du rôle de modèle de vers sans avoir à modifier `Bottles`.

### Obeying the Law of Demeter

Considérons le code suivant: 

```javascript
 class Bottles {
 // ...
 verse(number) {
 return new this.verseTemplate(number).lyrics();
 }
}
```
A la ligne 4 ci-dessus, la méthode `verse` sait :
que `this.verseTemplate` est un constructeur qui peut être appelée avec `new`
que `new` attend un argument
que l'argument de `new` doit être un nombre
que l'objet renvoyé par `new` ...(number) répond au message `lyrics`
que `lyrics` renvoie les paroles qui nous intéressent.

Cette liste énumère de nombreuses choses que `Bottles` connaît mais ne contrôle pas, ce qui signifie qu'il s'agit de dépendances. Les dépendances sont des vulnérabilités - si leur propriétaire les modifie, les effets de ce changement se répercuteront sur `Bottles`, qui pourrait alors être obligé de changer à son tour. Les dépendances ne peuvent pas être évitées mais doivent certainement être minimisées. 

Aucune de ces dépendances sont bonnes, mais une en particulier mérite notre attention. 
Ce code viole la `Loi de Demeter`: `Bottles` connait le collaborateur (`lyrics`) de son collaborateur (`new verseTemplate`) (si on considère que `new` est un message).
Conséquences: dur à tester! On doit créer une chaine d'objet..Couplage terrible :(

**Law of Demeter**
: Au sein d'une méthode, les messages ne doivent être envoyés qu'aux objets qui sont passés en tant qu'arguments à la méthode et aux objets qui sont directement accessibles à soi-même

Attention cependant car c'est plus subtile: la loi s'intéresse aux API des objets retournés, et non de chaque objet individuel. 
Ainsi ce code 

```javascript
'AbCdE'.repeat(2).replace('C', '!').toLowerCase().slice(1) // -> 'b!deabcde'
```

contient de nombreux points mais ne constitue pas une violation de Demeter car chaque message intermédiaire renvoie un objet conforme à la même API. Ce n'est pas le nombre de points qui importe, mais le type d'objet renvoyé par chaque message. 
La loi de Déméter limite effectivement la liste des autres objets auxquels un objet peut envoyer un message. Son but est de réduire le couplage entre les objets. Du point de vue des expéditeurs de messages, un objet peut parler à ses voisins mais pas aux voisins de ses voisins. Les objets ne peuvent envoyer des messages qu'à des collaborateurs directs. 

### Pushing Object Creation to the Edge

Les applications orientées objet bien conçues se composent d'objets faiblement couplés qui s'appuient sur le polymorphisme pour varier le comportement. L'injection de dépendances desserre le couplage. Le polymorphisme isole les comportements variables dans des ensembles d'objets interchangeables qui se ressemblent de l'extérieur mais se comportent différemment à l'intérieur. Du point de vue de l'objet dont les collaborateurs sont injectés, c'est parfait. Comme ces objets recevant l'injection peuvent interagir avec des collaborateurs nouveaux et inattendus sans avoir à changer, ils sont extrêmement flexibles. Vous avez besoin d'une nouvelle variante ? Il suffit de créer une nouvelle classe pour représenter cette variante et de l'injecter. Le récepteur n'en sait rien et ne s'en soucie pas.

Cependant, cette façon de penser à l'OO introduit une préoccupation qui n'a pas encore été pleinement abordée. Où ces objets injectés sont-ils créés ? Quand ? Et par qui ? Les applications qui utilisent l'injection de dépendances évoluent, naturellement et par nécessité, vers des systèmes où la création d'objets commence à se séparer de leur utilisation. La création d'objet est poussée plus vers les bords, vers l'extérieur, et les objets eux-mêmes interagissent davantage vers le vers le milieu, ou l'intérieur.

### Contrasting Unit and Integration Tests

Avant de rentrer dans les définitions, voici le status des tests pour la classe `Bottle` avant les refactos et donc avant qu'on en extrait des `BottleNumber`
et `BottleVerse`.
```js
 describe('Bottles', () => {
  test('the first verse', () => {
  const expected =
  '99 bottles of beer on the wall, ' +
  '99 bottles of beer.\n' +
  'Take one down and pass it around, ' +
  '98 bottles of beer on the wall.\n';
  expect(new Bottles().verse(99)).toBe(expected);
  });
```
Pour l'auteur, ce sont des tests unitaires qui se sont transformés au cours de la refacto en des tests d'intégration qui couvrent les classes `Bottle`, `BottleNumber` et `BottleVerse`


Les **tests unitaires** sont destinés à tester l'API publique d'une seule classe. L'unité testée nécessite souvent quelques objets collaborateurs pour fonctionner, mais ces autres objets sont tangents et n'existent que pour que vous puissiez traiter l'unité qui vous intéresse. Les tests unitaires ne doivent pas tester les collaborateurs.
Les **tests d'intégration** sont destinés à prouver que des groupes d'objets collaborent correctement ; ils montrent qu'une chaîne entière de comportements fonctionne.

Votre approche générale des tests doit être de créer un test unitaire pour chaque classe, et de tester chaque méthode de l'API publique de cette classe. méthode de l'API publique de cette classe. 



### Foregoing Tests



Comme indiqué précédemment, _vous devez aborder les tests avec l'intention de créer des tests unitaires pour chaque classe._ 
Après une longue et minutieuse réflexion, vous pouvez décider de permettre aux tests unitaires d'un objet de couvrir également un collaborateur, mais c'est l'exception. Vous devez prévoir d'écrire un test unitaire pour chaque classe, et vous êtes en droit d'attendre de voir un test unitaire pour chaque classe dans le code de quelqu'un d'autre. Les tests Bottles fournissent une couverture complète pour toutes les classes existantes, mais ce sont en fait des tests d'intégration qui se font passer pour des tests unitaires. Les faux tests unitaires qui couvrent des collaborateurs éloignés deviennent de moins en moins utiles pour le prochain programmeur. 
Lorsqu'un test implique la combinaison de nombreux objets, le code peut se casser assez loin de l'origine du problème. Il est alors difficile de déterminer la cause d'une erreur. 

Les tests d'intégration sont importants, mais ils ont un objectif différent de celui des tests unitaires. Les tests d'intégration sont parfaits pour prouver l'exactitude de la collaboration entre les groupes d'objets. Ils démontrent le fonctionnement global de l'ensemble ou d'un sous-ensemble de votre application. Comme ils couvrent beaucoup de terrain, ils sont souvent lents. 

En revanche, les tests unitaires s'adressent à vous, le programmeur. Ils vous aident à écrire, à communiquer le comportement attendu, à prévenir la régression et à déboguer de plus petites unités de code. Lorsque quelque chose ne va pas, ce sont les tests unitaires qui fournissent un message d'erreur près de la ligne de code incriminée. Comme ils réduisent l'ensemble des coupables potentiels du code à l'origine d'un problème, ils facilitent le débogage. 
Ils ne doivent pas vous gêner dans l'écriture du code, ce qui signifie qu'ils doivent être rapides. 

On ne saurait trop insister sur le fait que la plupart des classes méritent leurs propres tests unitaires explicites. Cela devrait être votre point de vue par défaut._Mais, tout comme chaque règle est censée être brisée, il arrive que la chose la plus utile à communiquer au sujet d'un objet soit qu'il est si petit, simple ou invisible que le fait de le tester individuellement augmenterait les coûts au lieu de les réduire_. De temps en temps, il est judicieux de tester un objet en conjonction avec l'unité qui l'englobe. Cela crée un test qui s'infiltre dans l'espace entre l'intégration et l'unitaire, mais si vous considérez les petits objets comme étant privés, vous pouvez être justifié d'appeler l'ensemble un test unitaire. _Les classes de la hiérarchie BottleNumber sont des exemples de choses si petites, si simples et si invisibles qu'elles ne méritent peut-être pas leurs propres tests unitaires._ Si vous souscrivez au principe selon lequel les applications doivent avoir une couverture de test de 100 %, vous devriez peut-être réexaminer votre définition de cette règle. Peut-être cela signifie-t-il "100% du code doit être exercé lors des tests unitaires", plutôt que "100% des méthodes publiques doivent avoir leurs propres tests personnels".

Les tests doivent vous donner la liberté d'améliorer le code, et non vous coller à son implémentation actuelle. Lorsqu'ils contraignent plutôt qu'ils ne libèrent, demandez-vous s'ils en valent la peine et envisagez de les omettre. Dans le cas rare où vous décidez de ne pas donner à une classe son propre test unitaire, vous devez être en mesure de défendre cette décision avec une justification clairement articulée. 

Considérons ce code: 

```javascript
class BottleVerse {
  static lyrics(number) {
  return new BottleVerse(BottleNumber.for(number)).lyrics();
  }
 
  constructor(bottleNumber) {
  this.bottleNumber = bottleNumber;
  }
 
 lyrics() {
 return (
 capitalize(`${this.bottleNumber} of beer on the wall, `) +
 `${this.bottleNumber} of beer.\n` +
 `${this.bottleNumber.action()}, ` +
 `${this.bottleNumber.successor()} of beer on the wall.\n`
 );
 }
}
```

Cette encapsulation de `BottleNumber` par `BottleVerse` signifie que si vous vous tenez dans l'espace entre les objets publics de cette application, vous ne verrez jamais une référence à `BottleNumber`. Cet encapsulation de `BottleNumber` par `BottleVerse` signifie que si vous vous tenez dans l'espace entre les objets publics de cette application, vous ne verrez jamais une référence à `BottleNumber`. Il est tellement invisible pour les observateurs extérieurs qu'il pourrait tout aussi bien ne pas exister.
En résumé, les `BottleNumbers` sont :
- petits
- simples
- invisibles de l'extérieur de `BottleVerse`
- utilisés dans aucun autre contexte que le `BottleVerse`

Ces facteurs se combinent pour suggérer que, au moins pour le moment, vous devriez considérer les `BottleNumber` comme une partie intégrante de  `BottleVerse`, et les tester dans le cadre du test unitaire de `BottleVerse`.

Par ailleurs, il se peut que les `BottleNumbers` deviennent plus compliqués si bien qu'ils devraient être testés indépendamment. 

Un dernier point avant de continuer. La partie la plus critique et la plus compliquée des numéros de bouteilles est la factory. Il est important de tester ses responsabilités, et non les responsabilités des objets qu'elle fabrique. Le travail de la fabrique est de prendre un nombre et de le transformer en une instance de la bonne classe. Ainsi le test pourrait ressembler à ceci :

```javascript
describe('BottleNumber', () => {
 test('returns correct class for given number', () => {
 // 0,1,6 are special
 expect(BottleNumber.for(0).constructor).toBe(BottleNumber0)
 // etc.
 // Other numbers get the default
 expect(BottleNumber.for(3).constructor).toBe(BottleNumber)
 // etc.
 });
});
```

## Reorganizing Tests

 Les tests unitaires doivent raconter une histoire éclairante. Ils doivent démontrer et confirmer les responsabilités directes de la classe, et ne faire rien d'autre. Vous devez vous efforcer d'écrire les tests les plus rapides possibles, avec le moins de tests possibles, en utilisant les attentes les plus révélatrices d'intentions, et en utilisant le moins de code possible.

 ## Summarize

 Un code très concret et étroitement couplé résistera aux changements de demain. Le code qui dépend d'abstractions faiblement couplées les encouragera. Parce que les tests doivent exécuter le code, ils fournissent des informations précoces et directes sur une conception inadéquate,
 Lorsque les tests sont difficiles à écrire, qu'ils nécessitent beaucoup de configuration ou qu'ils ne peuvent pas raconter une histoire satisfaisante, quelque chose ne va pas. Écoutez. Corriger les problèmes maintenant n'est pas seulement moins cher que de les corriger plus tard, mais améliorera votre code, clarifiera vos tests, et rendra votre travail heureux.