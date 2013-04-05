---
layout: post
title: "Reality is not a"
description: ""
category: 
tags: []
---
{% include JB/setup %}


Reality is not a ___ 

On ne peut pas énoncer grand chose sur la représentation du monde, on veut que des représentations possèdent des propriétés interressantes et c'est probablement tout. Tous les modèles de ce coté là, parceque suffisament complexes, approchent et atteignent très vite leurs limites (cf le théorème d'incomplétude ou le théorème du cygne noir).

Par contre, on oublie souvent que l'observation du monde est un arbre et aussi que cette observation a un schéma (schemalater, mais toujours un schéma). 

Pour aider à comprendre la différence entre la représentation du monde et l'observation du monde, je vous propose un peu de Scala.

Ceci est un block qui renvoit la valeur "MONDE" :
{% highlight scala %}
{
	val a = "monde"

	a.toUpperCase
}

{% endhighlight %}



Voici l'arbre syntaxique abstrait (simplifié) de ce block :
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


On a observé le monde (ici le code Scala) comme un arbre. Une des représentations possibles est un graphe où ```newTermName("a")``` est un noeud, lié à Literal(Contstant("monde")), à l'application avec la fonction "toUpperCase" etc ...

Ce n'est qu'une représentation du monde possible, basé sur ces observations. Il y a pleins d'autres, certaines peuvent même être compatible entre elles.

