---
layout: post
title: "Reality is a"
description: ""
category: 
tags: []
---
{% include JB/setup %}


Lorsque l'on parle de la réprésentation du monde, certains disent que l'on peut/doit le réprésenter comme des relations, d'autres comme un graphe, mais en fait non, il faut utiliser des relations logiques avec une ontologie. Et en fait pas du tout.

Ceux qui ont une forte afinité avec les mathématiques savent qu'il y a des problèmes majeurs avec la représentation du monde.

On ne peut rien affirmer sur la représentation du monde, on veut que certaines représentations possèdent des propriétés interressantes, mais on est loin de l'universalité.

Par contre, l'observation du monde est un arbre, l'observation du monde a un schema.

C'est ce que font ceux qui font de l'event sourcing ou des compilateurs.

Pour vous aider à comprendre la différence, je vous propose un peu de Scala. 

Ceci est un block qui renvoit la valeur "MONDE"
{% highlight scala %}
{
	val a = "monde"

	a.toUpperCase
}

{% endhighlight %}



Voici l'arbre syntaxique abstrait (simplifié) de ce block
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


Vous voyez le ```newTermName("a")``` ? J'ai observé mon monde comme un arbre, mais une des représentations possibles est un graphe où ```newTermName("a")``` est un noeud, lié à Literal(Contstant("monde")), à l'application avec la fonction "toUpperCase" etc ...

Ce n'est qu'une représentation du monde possible, basé sur ce cette observation. Il peut y avoir d'autres représentations valables qui peuvent cohabiter entre elles.



