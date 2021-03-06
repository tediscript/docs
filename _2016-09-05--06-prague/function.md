---
layout: base
title:  'Function words in UD v2'
---

# Function words in UD v2

One of the central design decisions of UD is to put priority on syntactic relations between content words and to treat function words essentially as "features" that attach to the content word they modify with special relations like [u-dep/aux](), [u-dep/cop](), [u-dep/mark]() and [u-dep/case](). We propose the following changes to the treatment of function words in v2:

* Remove the part-of-speech tag [u-pos/AUX]() (see [postags](postags.html) for discussion)
* Allow [u-dep/aux]() with other part-of-speech tags than [u-pos/VERB]() (see below)
* Remove [u-dep/auxpass]() from the universal relations (see [core dependents](core-dependents.html) for discussion)
* Limit languages to a single copula verb and use [u-dep/cop]() for all its uses (see [copula](copula.html) for discussion)
* Allow [u-dep/case]() with subordinate clauses when it has a clear case-marking function (see below)

## Nonverbal auxiliaries

<!-- Is aux only used with verbs, or are other parts of speech permitted, too? The current guidelines (http://universaldependencies.org/u/dep/aux_.html) just say "verbs". But the current data sometimes contain particles (Bulgarian) and maybe other categories as well. I think we have agreed that the infinitival particle "to/zu/att/..." should be mark and not aux, and it is in most of the treebanks (I fixed German a couple of days ago). But Bulgarian also includes a particle that marks the future tense, a function performed by auxiliary verbs in other languages. Is it allowed to be aux? -->

<!-- Joakim: In line with loosening the constraints between part-of-speech tags and syntactic relations, I don’t see a problem with allowing other things than verbs to have the aux relation. Maybe we should should define aux as a grammaticalized expression of TMA categories? -->

The v1 guidelines say that the [u-dep/aux]() relation is reserved for auxiliary _verbs_. However, some languages (for example Bulgarian, see example) use particles to construct periphrastic verb forms, hence we should also allow [u-pos/PART](). More generally: in line with loosening the constraints between part-of-speech tags and syntactic relations (see [here](postags.html) for discussion), we should define [u-dep/aux]() as a grammaticalized expression of tense-aspect-mood-voice-evidentiality categories.

~~~ sdparse
Като се прибереш, ще съм почистил къщата. \n When you return , will I.have cleaned the.house
aux(почистил, ще)
aux(почистил, съм)
~~~

Note that this does not necessarily mean that all non-verb aux dependents in the current data are correct. See [this query](http://bionlp-www.utu.fi/dep_search/query?search=%21%28AUX%7CVERB%29+%28%3Caux%7C%3Cauxpass%29+_&db=UD_English-dev) for an instance. They should be revised and each language-specific documentation should clearly state which lemmas may occur as auxiliaries and what TAMVE categories they are used in. That also applies to verbs—in some UD treebanks, the list of verbs that are attached as auxiliaries is very long and some of the verbs probably should not be `aux`.

## The use of case and mark with subordinate clauses

The [u-dep/case]() relation is used for adpositions and other particles that perform a case-marking function (in a wide sense). For example: 

~~~ sdparse
he depends on her
nmod(depends, her)
case(her, on)
~~~

The [u-dep/mark]() relation is used for subordinate conjunctions and other words that mark subordinate clauses (without having a referential function like relative pronouns). Thus:

~~~ sdparse
he believes that it is raining
ccomp(believes, raining)
mark(raining, that)
~~~

The assumption in v1 is that if the head is a nominal, only [u-dep/case]() can be used, and if the head is a clausal predicate, only [u-dep/mark]() can be used. However, applying this rule strictly means missing important generalizations
in languages that allow case-marking elements to attach to subordinate clauses. Consider the following Swedish 
examples:

~~~ sdparse
han tror henne \n he believes her
dobj(tror, henne)
~~~

~~~ sdparse
han litar på henne \n he trusts in her
nmod(litar, henne)
case(henne, på)
~~~

~~~ sdparse
han tror att det regnar \n he believes that it is raining
ccomp(tror, regnar)
mark(regnar, att)
~~~

~~~ sdparse
han litar på att det regnar \n he trusts in that it is raining
advcl(litar, regnar)
mark(regnar, att)
case(regnar, på)
~~~

The function of the preposition "på" is exactly the same in the second and fourth example, namely to convert a structure that can occur in a core role ([u-dep/dobj](), [u-dep/ccomp]()) to a structure that occurs in an oblique role 
([u-dep/nmod](), [u-dep/advcl]()). This function is completely distinct from the function of "att" in the third and fourth example, namely to convert a main clause structure into a subordinate or embedded clause. If we insist that "på" must be labed [u-dep/mark]() in the fourth example, because it attaches to a clausal predicate, then its function will be misrepresented.

