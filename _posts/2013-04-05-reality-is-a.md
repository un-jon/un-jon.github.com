---
layout: post
title: "Reality is not a ____"
description: ""
category: 
tags: []
---
{% include JB/setup %}


Il est difficile d'énoncer quelque chose sur la représentation du monde, on veut que des représentations possèdent des propriétés intéressantes et c'est probablement tout. Tous les modèles de ce coté-là, parce que suffisament complexes, approchent et atteignent très vite leurs limites (cf le théorème d'incomplétude ou le théorème du cygne noir).

Par contre, on oublie souvent que l'observation du monde est un arbre et aussi que cette observation a un schéma ("SchemaLater" et non "SchemaLess", mais toujours un schéma). 

Pour aider à comprendre la différence entre la représentation du monde et l'observation du monde, je vous propose un peu de Scala.

Ceci est un block qui renvoit la valeur "MONDE" :
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


La représentation classique du block ici, après les différentes phases du compilateur, ressemble à du bytecode pour la JVM :

{% highlight scala %}
Constant pool:
  #14 = Utf8               ()Ljava/lang/String;
  #15 = Utf8               Monde
  #16 = String             #15            //  Monde
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


Une autre des représentations possibles est un graphe où ```newTermName("a")``` est un noeud, lié à ```Literal(Constant("monde"))```, à l'application avec la fonction```"toUpperCase"``` ....

![cool dag](/assets/reality.dot.png)


Il y en a pleins d'autres, certaines peuvent même être compatible entre elles.

