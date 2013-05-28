---
layout: post
title: "Data Vs. Code"
description: ""
category: 
tags: []
---
{% include JB/setup %}


La semaine dernière j'ai fait sur talk sur la persistance en Scala au PSUG : http://psug-33.eventbrite.fr/ . C'était l'occasion d'aborder un sujet bien plus important à mon goût que le choix entre les technologies de gestion de mémoires secondaires partagées : la conception et l'utilisation de réprésentations intermédiares. 

S'il y avait certaines choses à retenir, les voici : 

* Data is Code, Code is Data (définition dite algébrique, généralisation => machine de turing ...) , mais à un niveau d'abstraction donné, la donnée est quelque chose que l'on peut manipuler, toucher, sérialiser, désérialiser, envoyer sur le réseau . On ne peut pas faire la même chose avec le code au même niveau d'abstraction (définition dite analytique, c'est à dire liée à l'étude des particularités). 

* Un système se définit dans l'action, pas dans la façon où celui-ci se représente son extérieur (ou son intérieur). On n'espère pas d'un système qu'il agisse sans raisons, qu'il fasse preuve d'une certaine forme d'inteligence (ou surtout de bétise). La donnée, sous forme de documents, est là pour justifier les actions qui sont faites par un système, le code donne le sens qu'il faut aux documents pour provoquer ses actions. On peut donc dans un système regarder ce qu'il est en cours en inspectant les documents qu'il manipule ou qu'il vient de produire.

A ces deux concepts, on peut associer les définitions suivantes : 

* Système : Un intérieur et un extérieur ! On peut définir un système par la frontière que celui-ci denote entre un intérieur et un extérieur, on communique avec un système par des documents qui passe à travers cette frontière, la nature des documents qui passe à travers cette frontière définit un protocol. Un système peut être un groupe de sous système (all the way down).

* Document : Données ayant la granularité suffisante pouvant servir de justification à une action. La notion de granularité suffisante est importante, cela permet par exemple d'aporter une réponse plus concrète à la granularité des représentations exposées dans la Web Oriented Architecture.  Par exemple, dans son talk sur la WOA H. dit que cette granularité est culturelle, je pense qu'elle n'est pas que culturelle, mais très liée à la façon dont un système utilise les documents, la granularité va être celle qui permet de justifier des actions. Une granularité plus petite ne sert à rien, il va falloir faire tout de suite des agrégats, si une granularité plus grande il va falloir préindexer, prétransformer les documents.

* Représentation intermédiaire : Un document amène une représentation (de lui-même ou de quelque-chose (aboutness)). Cette réprésentation est intermédiaire car elle peut amener plusieurs interprétations ou plusieurs représentations. Chaque système peut avoir une interprétation différente, mais une représentation intermédiaire commune. Par exemple un compilateur peut avoir la même représentation qu'un analyseur statique de code, l'un va créer à partir de cette représentation un programme exécutable, l'autre va pouvoir par exemple un call graph. Le concept d'interprétation de représentation est lié à certaines approches linguistiques. En linguistique, pour un symbol, il y a le signifiant (grosso modo la représentation concrète du symbol exemple la lettre grecque lambda : λ ) et le signifié (TODO).
Un groupe de système peut donc partager les mêmes signifiant (ici la même lettre grecque), mais avoir différents signifiés.

Les systèmes peuvent au cours du temps changer la façon dont ils interprêtent cette représentation intermédiare (exemple, amélioration d'un moteur de gestion de base de données) 

* Epoché (prononciation, Epoqué) : Epoché une suspension du temps et du jugement, c'est un concept assez pratique pour faire la conception d'une représentation intermédiaire.

Une représentation intermédiaire peut être poluée par des signifiés, par exemple, je décris les canaries : 

```clojure
{:taille-moyenne #unit/meter 0.20
 :nom-par-défaut "Titi"}
```

et j'ajoute à cette décription certains éléments (Il dit qu'il ne voit pas le rapport) : 

```clojure
{:capitaine "José le Nantais"}
```


Maintenant, j'ai une représentation intermédiaire qui est polué d'un complément d'information. On pourrait l'ignorer, mais ce n'est pas pratique car cette information complémentaire est présente dans le document au même niveau que les autres parties du document.

Un autre exemple serait de stocker l'appel d'une fonction avec son résultat : 

```clojure
{:appel '(+ 1 1) 
 :resultat 42 }
```

Il faut donc lors de création de document limiter les inférences, les compléments, les déductions "hative" ou la platonification du monde (prenez ce que vous voulez). Il va falloir mettre dans le document que le stricte minimum, c'est à dire le contexte de création du document et juste les informations observées, par exemple :

```clojure
(user/login 
  {:time #inst "2013-05-22 ..."
   :user "Jon"
   :computer "Le MBP avec des gommettes"}
)
```

Cette limitation ne veut pas dire qu'il faut se passer de document contenant , on peut avoir dans le même système : 

```{:appel '(+ 1 1)}``` et 
```{:appel '(+ 1 1) :resultat 42}```

Mais ```{:appel '(+ 1 1) :resultat 42}``` n'est pas le document maître [1].

Et c'est tout pour cette-fois ci. Voici quelques pointeurs pour des éléments connexes : 

- L'article de Pierre Chapuis (Algorithms, Data Structures and Protocols) :  http://blog.separateconcerns.com/2013-01-28-algorithms-data-structures-protocols.html

- Les videos de Rich Hickey : http://www.infoq.com/presentations/Value-Values , http://www.infoq.com/author/Rich-Hickey
 
- Mme Clarisse ? http://www.parleys.com/play/51703640e4b095cc56d8d4af/chapter1/about


 [1] C'est une sujet à développer dans le cadre des bdd graphes (forme de bdd que j'aprécie tout de même), est-ce que c'est génant de stocker l'index (l'index primaire d'un graphe est le graphe), la data, les documents et dark vador dans le même espace muable et non versionné ? C'est probablement pratique, mais je ne vois pas comment on pourrait se passer dans ce cas là d'une bonne bande qui stocke la donnée maître (au cas où ...).
