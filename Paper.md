Birth of withFilter
========================

Motivation: Why do people want this?

Rejected ideas: What were some other solutions?

Accepted idea: Why was withFilter accepted when other things weren't?

Implementation: Maybe go into the implementation, uses flat map so this is relevent. Also, maybe check to see if it has been changed in more recent revisions

Erik leiks words:
=================

Users new to Scala are often confused by the many nuances inherent in functional programming.  Complicated expressions and datatypes in analogous imperative languages are reduced to mere lines of code, the mechanics of which lie hidden behind the elegance of recursive statements with functions as values.  The filter and withFilter classes are ideal examples of this high-level abstraction.  Their respective implementations appear essentially identical, and both will produce the same results.  

--figure--
<put a list being filtered and mapped here>
<put a list being withFiltered and mapped here>
--figure--

Scala did not originally include the class withFilter but instead was added as a result of community discussion (cite sources).  It offers performance benifits over filter under certain circumstances, extends a different class in the type hierarchy (cite scala-lang.org), and even returns a different value type.  These characteristics first appear counter-intuitive, given how similiar their code is to use in practice.  In highlighting the differences between the classes the question is not what the filters are doing, or when they are used, but in how the are implemented.  

Filter extends the transformer class and works by applying some conditional over all elements of a list.  This conditional can come in a variety of flavors, from simple comparison statements to complicated functions.  Applying multiple filters to a list results in a chaining process which requires the list to be iterated over per-filter.  This results in a complexity of n^m, where n is the size of the list and m is the filter total (cite source).  As lists become large, and the chaining becomes verbose, there is potentially a very costly computation associated with use of the filter class.  

withFilter differs very subtley from its counterpart by a change in its return type.  Like filter, it applies a conditional over a list, but cannot traverse a list by itself to perform that filtering.  Instead, withFilter requires a map, flatmap, or foreach statement to act on its return value. (more apply?) When this statement then iterates over the list the withFilter gets applied to each element as it does.  This is an enormous boon to performance, as it ensures that the list is traversed only once, no matter how complex of a filter is used.

--output samples--
scala> testlist.withFilter(x => (x%2 == 0 )).map(x => x)
res3: List[Int] = List(10, 20, 30, 50)

scala> testlist.withFilter(x => (x%2 == 0 ))
res4: scala.collection.generic.FilterMonadic[Int,List[Int]] = scala.collection.TraversableLike$WithFilter@257db177

scala> testlist.filter(x => (x%2 == 0 )).map(x => x)
res5: List[Int] = List(10, 20, 30, 50)

scala> testlist.filter(x => (x%2 == 0 ))
res6: List[Int] = List(10, 20, 30, 50)
--output samples--

PS:  I'm not sure what benefit this really has with respect to guards.  MOAR RESEARCH NEEDED <3



Resources:
=========================

[Motivation and discussion][] Mailing list discussion resulting in withFilter

[Implementation][] Code revision with newly implemented withFilter

[Example with for][] Excellent example from stackoverflow (wtf it duz)

[General example][] Uses little words, tries to explain the differences. 

[Motivation and discussion]:http://scala-programming-language.1934581.n4.nabble.com/Rethinking-filter-td2009215.html

[Example with for]: http://stackoverflow.com/a/1059501

[General example]: http://tataryn.net/2011/10/whats-in-a-scala-for-comprehension/

[Implementation]:https://code.google.com/p/scalacheck/source/diff?spec=svn506&r=506&format=side&path=/trunk/src/main/scala/org/scalacheck/Gen.scala
