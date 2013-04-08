---
layout: post
title: "Reality is not a ____"
description: ""
category: 
tags: []
---
{% include JB/setup %}


Avec la mouvance NoSQL, cela fait maintenant quelques temps que l'on entend un peu partout que le modèle relationnel est trop pauvre (pour ceux qui offrent un modèle plus riche) ou trop riche (pour ceux qui offrent un modèle plus pauvre). 

Et donc en fonction de l'usage prévu, il va falloir choisir à l'inception d'un projet le modèle que l'on va utiliser pour représenter le domaine et le système de gestion de ce modèle qui va avec [^1].

Bravo, et comme nous sommes world class modeler (si vous ne l'êtes pas, vous allez le devenir), nous allons tordre la donnée de notre domaine pour qu'elle rentre dans le modèle choisi en optimisant selon quelques critères : 
 - La minimisation du nombre de ____.
 - La maximisation du nombre de résultat sans ____ [^2] . 
 - un ou deux critéres en fonction de ce qu'il y a à la mode en ce moment.

Et c'est très intéressant comme méthode, c'est comme ça que l'on s'est retrouvé avec 40ans de SQL, pour :
- minimiser la place nécessaire pour le modèle,
- maximiser le nombre de résultat sans inférence (jointure)

Quand le modèle fut bien déployé, on a maximisé la réutilisation d'un modèle déjà connu ...

On connaît l'histoire et elle n'est pas forcement mauvaise.


L'histoire que l'on a oublié, c'est celle de la mécanisation de la pensée du début du 20ème siècle, les travaux sur les mathématiques ainsi que des couperets qui s'en suivent comme l'imcomplétude de Gödel et autres.

Gödel montre que tout système formel sufisament complexe est imcomplet, la course à la modélisation est infinie. Plus on complexifie le modèle, plus il faut le complexifier pour qu'il gère ses propres éléments.

Il ne sert à rien de vouloir obtenir des modèles complets, il faudrait minimiser les modèles.

## Mon rasoir d'Occam

La minimisation des modèles n'est pas suffisante, les modèles que l'on peut créer pour telle ou telle problèmatique sont faux, souvent peut évolutif [^3]. On ne peut pas se permettre de passer 3 mois à se poser les questions si oui ou non on versionne une partie du modèle, quel est la cardinalité entre plusieurs entités, etc etc.

A mon avis, le problème global avec les modèles, c'est qu'ils sont responsables de trop de choses à la fois, et en particulier :
- d'accumuler les observations sur le domaine modélisé 
- et de fournir une représentation.

Les représentations sont systèmatiquements fauses (mais parfois utiles), et la façon de faire des observations changent avec le temps. Par contre les observations sont 'vraies' au moment où elles sont faite.

Si l'on veut évoluer dans la modèlisation, **il faut faire la ségrégation de l'observation du domaine dans les modèles, des représentations.**


## Un exemple avec du "code"

Pour aider à comprendre la différence entre l'observation dans un modèle et la représenation, je vous propose un peu de Scala.

Ceci est un block qui renvoit la valeur "MONDE" (un block en Scala retourne la dernière expression).

{% highlight scala %}
{
	val a = "monde"

	a.toUpperCase
}

{% endhighlight %}



Voici l'arbre syntaxique abstrait (simplifié) de ce block  (Observation du block) :
{% highlight scala %}
Block(
    // définition de la valeur
	ValDef(
		newTermName("a")
		, Literal("monde")
	)
	// application de la méthode
	, Apply(
		newTermName("a")
		, newTermName("toUpperCase")
	)
)
{% endhighlight %}


Voici la représentation classique du block ici pour l'execution, après les différentes phases du compilateur :

{% highlight scala %}
Constant pool:
  #14 = Utf8               ()Ljava/lang/String;
  #15 = Utf8               monde
  #16 = String             #15            //  monde
  #17 = Utf8               LineNumberTable
  #18 = Utf8               java/lang/String
  #19 = Class              #18            //  java/lang/String
  #20 = Utf8               toUpperCase
  #21 = NameAndType        #20:#14        //  toUpperCase:()Ljava/lang/String;
  #22 = Methodref          #19.#21        //  java/lang/String.toUpperCase:()Ljava/lang/String;
{
    Code:
      stack=1, locals=1, args_size=1
         0: ldc           #16                 // String Monde
         2: astore_1
         3: aload_1
         4: invokevirtual #22                 // Method java/lang/String.toUpperCase:()Ljava/lang/String;
         7: areturn
}
{% endhighlight %}


Et voici une autre des représentations possibles est un graphe où ```newTermName("a")``` est un noeud, lié à ```Literal(Constant("monde"))```, à l'application avec la fonction```"toUpperCase"``` ....

![cool dag](/assets/reality.dot.png)


(On peut aller voir du coté de Codeq pour ça, c'est très intéressant : http://blog.datomic.com/2012/10/codeq.html )




On a ici à partir d'une seule observation du code Scala (l'AST), dont les noeuds sont indépendants entre eux (il n'y pas plus d'informations que la structure et le contenu de chaque noeud), deux représentations ad hoc du monde pour des usages différents.

Ici, on a séparé le modèle du code (AST) de ses représentations pour des usages particuiers (execution / analyse).



## Le cas des compilateurs et langages









[^1]: Je pense sincérement que beaucoup font le saut du sol, et passe directement au "choix" du système de gestion. Les vendors de toutes les époques sont doués pour le FUD.

[^2]: inférence, prodstoc, applicatif custom, map reduce, ...

[^3]: c'est une bonne chose, la complexité vient de la paramétrisation
