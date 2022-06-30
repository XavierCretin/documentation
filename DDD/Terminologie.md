# [DDD](https://docs.microsoft.com/en-us/previous-versions/msp-n-p/jj591560(v=pandp.10))
DDD est une approche du développement de systèmes logiciels, et en particulier de systèmes complexes, dont les règles de gestion changent constamment et qui sont destinés à durer à long terme dans l'entreprise.

Son objectif principal est de lutter contre la complexité des processus métier, leur automatisation et leur implémentation dans le code. 

# [Modèle du domaine](https://domaindrivendesign.org/the-main-goal-of-domain-driven-design/) (Domain model)

Concept clé du DDD, le modèle du domaine est un dictionnaire de termes du langage omniprésent.

Le modèle de domaine et le langage omniprésent sont tous deux limités à un contexte, qui, dans le cadre du DDD, est appelé un *bounded context* (contexte délimité).

# [Langage omniprésent](https://docs.microsoft.com/en-us/previous-versions/msp-n-p/jj591560(v=pandp.10)#ubiquitous-language) (Ubiquitous language)

Cet autre concept clé de langage omniprésent est très étroitement lié à celui de modèle de domaine. 

L'une des fonctions du modèle de domaine est de favoriser une compréhension commune du domaine entre les experts du domaine et les développeurs. 

Si les experts du domaine et les développeurs utilisent les mêmes termes pour les objets et les actions du domaine (par exemple, conférence, président, participant, réserve, liste d'attente), le risque de confusion ou de malentendu est réduit. Plus précisément, si tout le monde utilise le même langage, il y a moins de risques de malentendus résultant de traductions entre langues.

# [Entités, objets-valeur, et services](https://docs.microsoft.com/en-us/previous-versions/msp-n-p/jj591560(v=pandp.10)#entities-value-objects-and-services) (Entities, value objects, and services)

DDD utilise les termes suivants pour identifier certains des artefacts internes (on parlera aussi de *outils tactiques*) qui constitueront le modèle de domaine.

## Entités (Entities) 

Les entités sont des objets qui sont définis par leur identité, et cette identité se poursuit dans le temps. Par exemple, une conférence dans un système de gestion de conférence sera une entité ; nombre de ses attributs peuvent changer au fil du temps (comme son statut, sa taille et même son nom), mais au sein du système, chaque conférence particulière aura sa propre identité unique. L'objet qui représente la conférence n'existe pas toujours dans la mémoire du système ; il peut parfois être conservé dans une base de données, pour être réinstancié à un moment donné dans le futur.

## Objet-valeur (value object)

ous les objets ne sont pas définis par leur identité. Pour certains objets de valeur, ce sont les valeurs de leurs attributs qui sont importantes. Par exemple, dans notre système de gestion des conférences, nous n'avons pas besoin d'attribuer une identité à l'adresse de chaque participant (l'une des raisons est que tous les participants d'une organisation donnée peuvent partager la même adresse). Tout ce qui nous intéresse, ce sont les valeurs des attributs d'une adresse : rue, ville, état, etc. Les objets-valeur sont généralement immuables.

## Services

Vous ne pouvez pas toujours tout modéliser comme un objet. Par exemple, dans le système de gestion des conférences, il peut être judicieux de modéliser un système de traitement des paiements tiers comme un service. Le système peut transmettre les paramètres requis par le service de traitement des paiements, puis recevoir un résultat en retour du service. Notez que la caractéristique commune d'un service est qu'il est sans état (contrairement aux entités et aux objets de valeur).

# [Agrégat et agrégat racine](https://docs.microsoft.com/en-us/previous-versions/msp-n-p/jj591560(v=pandp.10)#entities-value-objects-and-services) (Aggregate and aggregate root)
 

Un Agrégat est un groupe d'entités et d'objets-valeur associés qui sont considérés comme un tout unique vis-à-vis des modifications de données.

Dans le monde du DDD, c'est un pattern qui permet de définir l’appartenance et les frontières des objets.

Eric Evans dans *What I've learned about DDD since the book*
> "Un agrégat, c'est comme du raisin, dans le sens où vous avez quelque chose que vous considérez comme un tout conceptuel, qui est également constitué de parties plus petites. Vous avez des règles qui s'appliquent à l'ensemble." 

Une **racine d'agrégat** (également appelée entité racine) est l'objet gardien de l'agrégat. 

Tous les accès aux objets de l'agrégat doivent se faire par l'intermédiaire de la racine de l'agrégat ; les entités externes ne sont autorisées à détenir qu'une référence à la racine de l'agrégat et tous les invariants doivent être vérifiés par la racine de l'agrégat.

Si la racine est supprimée et retirée de la mémoire, tous les autres objets de l’agrégat seront supprimés aussi, car il n’y aura plus d’autre objet possédant une référence vers l’un d’entre eux. 

Si les objets d’un Agrégat sont stockés en base de données, seule la racine devrait pouvoir être obtenue par des requêtes. Ont devrait accéder aux autres objets à travers des associations traversables. 

L’Entité racine a une identité globale, et elle est responsable du maintien des invariants. Les Entités internes ont une identité locale. 