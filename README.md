# sura-features

This repository contains code utility files to extract features from suras of the Qur'an based on the words and themes that occur in them.

# Introduction

The question of whether the ordering of suras in the Qur'an is divinely ordained or arises out of a consensus of the Companions has been debated over the centuries. There are two main ways this question has been approached: 

- Extrinsically or textually/historically: Through Qur'anic or hadith evidence for a specific ordering. There does not appear to be slam-dunk evidence for either side, unfortunately.
- Intrinsically: Through showing that a particular ordering has an obvious, compelling eloquence and self-consistency to it, i.e. the "proof is in the pudding." Scholars such as Farahi and Islahi have presented arguments for divine ordainment based on ideas of نظم ("coherence"). 

We propose a computational method to explore this question combining elements from both approaches. Specifically, we propose ways to quantify the "self-consistency" or "coherence" of an ordering of suras. This will allow us to not only assert the remarkable coherence within a particular ordering, but to also assert "less" coherence in other orderings. Finally, we can use textual/historical evidence to validate these measures of ordering coherence.

# Approach

Our approach to defining self-consistency or coherence of a sura ordering centers around decomposing an ordering into an ordered set of sura pairs, and summing a "connectedness" measure for each pair in the set. For each pair of suras, we measure "connectedness" by analyzing their similarity and complementarity, on both syntactic and semantic levels. These are purely structural, word-based, and theme-based measures; there is no numerology involved.

For example, take Surat al-Isra' and Surat al-Kahf. In terms of aya length, they are one aya apart (111 vs 110) and in terms of page/line length they are ~5 lines apart. Each contains an aya about the refusal of Iblis to prostrate, about the examples detailed in the Qur'an, and other similar ayat. Thematically one opens addressing the Bani Isra'il and the other opens addressing Christians (complementarity). These, and others, will contribute to the final connectedness score of this pair of suras. We propose to define and compute this measure for all 114*113/2 = 6,441 pairs of suras.

## Sura and Pair Features

We will extract "features" from each sura based on the words and themes contained within it. These features will then be used to compute pair-wise connectedness.
In order to be as objective as possible, we define broad justifiable "classes" of features and include all features from within that class. Following this, we define a separate connectedness measure for _pairs_ of suras based on their individual features that accounts for their similarity and complementarity.

### Connectedness: Similarity and Complementarity

In coming up with a measure to define how connected a pair of suras are, we are looking for deep structural connections, beyond superficial similarities. For this reason, we include measures of _complementarity_ in connectedness. For example, if Sura 1 has an 80-20 distribution of ayat related to janna and nar, and Sura 2 has a 20-80 distribution, we take that as strong evidence of abstract, structural similarity between the two. Our measure of connectedness subsumes both similarity and complementarity.

## Features

In order to compute pair connectedness scores, we need to define an objective way of quantifying characteristics of words and themes in each sura.

### Sura Features

For each sura, we will extract two types of features: one set based on aggregate numerical statistics like word/aya/line count and another based on existence/absence of structural and thematic characteristics. See the Appendix for a complete list of sura features.

### Pair Features

# Validation

We can "validate" a feature set by treating groups of suras that we know to be connected to each other through textual/historical evidence as "test sets." For example, the Prophet (saws) spoke of "Hūd and her sisters" as a group of suras similar to one another. Similarly we know that the Prophet (saws) repeatedly recited certain pairs/groups of suras together. We also have suras confirmed to be revealed in the Makkan and Madinan periods of the sira. For each such group, we take a feature set as valid if in-group connectedness is higher than out-group connectedness.

# Related Work

## Differences with the Farahi/Islahi methodology

The main differences of this approach with the نظم ideas proposed by Farahi and Islahi are as follows:

1. We look at entire orderings of suras, not just individual pairs.
2. The question of ordering _within_ a pair is not of concern to us because an ordering specifies that completely.
3. We are able to deal with not only pairs, but _n_-tuples, including triplets and septets hypothesized by Farahi.
4. We compute coherence measures for all possible orderings, not just the one well-known one.

# Appendix

## Sura Features

Structure - e.g. chiastic

IDF Features, n-grams (unordered, synonyms)

unique features of each sura (high IDF words/names).  Sum of top 5 or average word uniquness?

Mean/median aya length

Repeated phrases (Qamar, Rahman, etc.)

## Sura pair features

Complementarity (e.g. in the topics themselves, or in brevity vs detail) in themes/subjects/`amud

Word distribution kl divergence 

End and beginning match (theme)? 

Beginning match. Letters or phrases similarity. 
