---
layout: post
title: "Reality is not a"
description: ""
category: 
tags: []
---
{% include JB/setup %}


Reality is not a ___ 

On ne peut rien énoncer sur la représentation du monde, on veut que certaines représentations possèdent des propriétés interressantes, mais c'est tout, tout les modèles de ce coté là approchent et atteignent très vite leurs limites (cf Gödel, le cygne noir).


Par contre, on oublie souvent que l'observation du monde est un arbre et aussi que l'observation du monde a un schéma (schemalater, mais toujours un schema). 

Pour vous aider à comprendre la différence entre la représentation du monde et l'observation du monde, je vous propose un peu de Scala.

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

Ce n'est qu'une représentation du monde possible, basé sur ces observations. Il peut aussi y avoir d'autres représentations valables qui peuvent cohabiter.

