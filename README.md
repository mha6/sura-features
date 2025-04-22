# sura-features

This repository contains code utility files to extract features from suras of the Qur'an based on the words and themes that occur in them.

# Introduction

The question of whether the ordering of suras in the Qur'an is divinely ordained or arises out of a consensus of the Companions has been debated over the centuries. There are two main ways this question has been approached: 

- Extrinsically or textually/historically: Through Qur'anic or hadith evidence for a specific ordering. There does not appear to be slam-dunk evidence for either side, unfortunately.
- Intrinsically: Through showing that a particular ordering has an obvious, compelling eloquence and self-consistency to it, i.e. the "proof is in the pudding." Scholars such as Farahi and Islahi have presented arguments for divine ordainment based on ideas of نظم ("coherence"). 

We propose a computational method to explore this question combining elements from both approaches. Specifically, we propose ways to quantify the "self-consistency" or "coherence" of an ordering of suras. This will allow us to not only assert the remarkable coherence within a particular ordering, but to also assert "less" coherence in other orderings. Finally, we can use textual/historical evidence to validate these measures of ordering coherence.

# Approach

Our approach to defining self-consistency or coherence of a sura ordering centers around decomposing an ordering into a set of sura pairs, and summing a "connectedness" measure for each pair in the set. For each pair of suras, we measure "connectedness" by analyzing their similarity and complementarity, on both syntactic and semantic levels. These are purely structural, word-based, and theme-based measures; there is no numerology involved.

For example, take Surat al-Isra' and Surat al-Kahf. In terms of aya length, they are one aya apart (111 vs 110) and in terms of page/line length they are ~5 lines apart. Each contains an aya about the refusal of Iblis to prostrate, about the examples detailed in the Qur'an, and other similar ayat. Thematically one opens addressing the Bani Isra'il and the other opens addressing Christians (complementarity). These, and others, will contribute to the final connectedness score of this pair of suras. We propose to define and compute this measure for all 114*113/2 = 6,441 pairs of suras.

## Sura and Pair Features

We will extract "features" from each sura based on the words and themes contained within it. These features will then be used to compute pair-wise connectedness.
In order to be as objective as possible, we define broad justifiable "classes" of features and include all features from within that class. Following this, we define a separate connectedness measure for _pairs_ of suras based on their individual features that accounts for their similarity and complementarity.

### Connectedness: Similarity and Complementarity

In coming up with a measure to define how connected a pair of suras are, we are looking for **deep structural connections**, beyond superficial similarities. For this reason, we include measures of _complementarity_ in connectedness. For example, if Sura 1 has an 80-20 distribution of ayat related to janna and nar, and Sura 2 has a 20-80 distribution, we take that as strong evidence of abstract, structural similarity between the two. Our measure of connectedness subsumes both similarity and complementarity.

## Features

In order to compute pair connectedness scores, we need to define an objective way of quantifying characteristics of words and themes in each sura.

### Sura Features

For each sura, we will extract three types of features: one set based on aggregate numerical statistics like word/aya/line count, another based on existence/absence of structural, audial, and thematic characteristics, and the last based on word occurrence and frequency in that sura as well as the rest of the Qur'an. We therefore envision a long, sparse vector as representing a sura, with three parts:

1. Aggregate numerical statistics:

   - Number of words/ayas/lines in the sura
   - Mean and median aya length in terms of words
   - Length distribution of ayat (bucketized?)
   - Average [word IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) and sum of top _k_ IDF values
   - Categories of top _k_ IDF value words (e.g. are they verbs, names of places, Prophets, etc.). The idea is to capture features of a sura that are unique to only it and few others.

2. Structural, audial, and thematic features:  
By topic in the list below, we mean theme, or subject, or _`amud_ (as in the Farahi/Islahi terminology).

   - Does the sura have a complete/partial structure? e.g. chiastic, etc. (Categorical)
   - Does the sura have notable rhyming characteristics in its recitation?
   - Are there any notable audial or tajwid characteristics of the sura?
   - What are the top _k_ topics in the sura and their distribution? (topics include items like story of a Prophet or a past nation, laws, janna, nar, etc.)
   - What is the noun/verb/particle distribution of the sura?
   - How does the sura begin? Syntax: With disconnected letters or _hamd_ or _tasbih_ or something else? Semantics: What topic does it open with?
   - Other structural features that are relatively unique to the sura, e.g. does it have an oft-repeated phrase like in Surat al-Rahman and Surat al-Qamar?

3. Bag-of-word features: We will use [inverse document frequency (IDF)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) features to represent a sura as a set of its constituent _unordered_ n-grams (n=1,2,3). We will collapse synonyms and words of the same root into one. This will quickly surface similar words/phrases (_mutashabihat_) that occur in that sura and possibly in a few others only.

See the Appendix for a complete list of sura features (TBD).

### Pair Features

For each _pair_ of suras we will compute values for the following components of connectedness and aggregate them into a final scalar value representing a custom connectedness measure that is derived from each sura's features.

 - Similarity of aggregate statistics
 - Similarity of structural features
 - Similarity of bag-of-word features
 - A symmetric probability divergence measure between topic distributions and other distributions that are part of sura features. This may need to be customized to handle complementarity (janna and nar, or brevity and detail on the same topic, for example).
 - Are the beginning and ending topics similar/complementary?
 - Are the beginnings of the two suras similar or complementary or neither (in syntax and in meaning)? 

# Validation

We can "validate" a feature set by treating groups of suras that we know to be connected to each other through textual/historical evidence as "test sets." For example, the Prophet (saws) spoke of "Hūd and her sisters" as a group of suras similar to one another. Similarly we know that the Prophet (saws) repeatedly recited certain pairs/groups of suras together. We also have suras confirmed to be revealed in the Makkan and Madinan periods of the sira. For each such group, a measure of that validity of a feature set is how well it separates in-group connectedness from out-group connectedness measures.

# Putting it all together

Once we have computed all pairwise connectedness scores between the suras and have a 114x114 matrix of values, finding the most connected ordering reduces to finding the weighted longest path in a fully connected graph, which is equivalent to the [Traveling Salesman Problem](https://en.wikipedia.org/wiki/Travelling_salesman_problem), an [NP-hard](https://en.wikipedia.org/wiki/NP-hardness) problem.

## Subjectivity

The choice of features and any implicit/explicit feature weighting in the final connectedness measure, of course, is quite subjective. It is possible that different feature sets corresponding to different notions of connectedness (or even a different concept) will prefer different orderings. This code package allows experimentation with different feature sets to explore how sensitive orderings are to features and choice of feature sets. We do not claim to definitively answer the question; this work simply provides a tool for exploration and quantification.

# Related Work

## Differences with the Farahi/Islahi methodology

The main differences of this approach with the نظم ideas proposed by Farahi and Islahi are as follows:

1. We look at entire orderings of suras, not just individual pairs.
2. The question of ordering _within_ a pair is not of concern to us because an ordering specifies that completely.
3. We are able to deal with not only pairs, but _n_-tuples, including triplets and septets hypothesized by Farahi.
4. We compute coherence measures for all possible orderings, not just the one well-known one.

# Appendix

## Sura Features

## Sura pair features