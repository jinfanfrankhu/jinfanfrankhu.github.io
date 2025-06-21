---
layout: project
title: Tokenization in Turkish and Finnish
image: /images/projects//tokenizationtf/tokenizationtf.png
description: Scraped text in Turkish and Finnish to study tokenization in agglutinative languages. Evaluated using Word2Vec models and Named Entity Recognition sets.
permalink: /projects/tokenizationtf/
---

![Animation of Word2Vec]({{ "/images/projects/tokenizationtf/Word2VecVisualization.gif" | relative_url }})

This is a 3d projection of 72,000 English words in Word2Vec. Every point represents a word, and the distance between them captures (or at least, tries to capture) semantic difference. 

#### So what? How do the points represent words?

TLDR: This is actually super relevant now, as this is an old model for how generative AI encodes words. AI models assign words large lists of numbers and do operations with those numbers to predict what to say. 

<details>
<img src="/images/cat-closed.png" style="cursor: pointer; display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>The idea behind Word2Vec is deceptively simple: words that appear in similar contexts tend to have similar meanings. Instead of trying to define what a word is, Word2Vec learns what words do based on how they behave and where they co-occur in real text. It slides a small window across text and learns to predict nearby words from a given word (or vice versa). Over time, it adjusts a set of numerical vectors so that words used in similar contexts end up with vectors that cluster together.</p>

<p>This process turns language into geometry. Words that appear in similar contexts often land near each other in the high-dimensional space, because they share linguistic environments, and therefore, meanings. More impressively, Word2Vec captures relationships as directions: the famous example is that the vector difference between <em>king</em> and <em>queen</em> roughly matches the difference between <em>man</em> and <em>woman</em>.</p>

<p><img src="{{ '/images/projects/tokenizationtf/VectorDifference.png' | relative_url }}" alt="Vector difference between uncle, aunt, man, and woman" style="display: block; margin: 20px auto; max-width: 100%; height: auto;"></p>

<p>So why does this matter for AI today? Modern large language models (LLMs) like GPT or Gemini are way more sophisticated than Word2Vec, but the core idea of representing words as vectors that encode meaning is still foundational.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

#### Ok, cool. So what does this have to do with Turkish and Finnish? 

TLDR: Turkish and Finnish build words by using lots of suffixes, and sometimes it's more efficient for AI's to break up words into smaller tokens (tokenization) to better understand the text.

<details>
<img src="/images/cat-closed.png" style="cursor: pointer; display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>While English typically forms meaning through relatively short, separate words, Turkish and Finnish are highly agglutinative languages: they build words by attaching multiple suffixes to a root, sometimes stacking many layers of grammatical information into a single long word. This creates challenges for language models. If you tokenize purely at the word level, many unique word forms appear extremely rare or entirely unseen in training data, even though the underlying root or morphemes are quite common.</P>

<p>"Evlerimizde" = "In our houses"</p>
<p>"ev" = house, "ler" = plural, "imiz" = our, and "de" = in</p>

<p>By breaking words into smaller subword units, models can better generalize across these variants. Going back to Word2Vec and other similar embedding models, all of these systems are heavily affected by tokenization because the "units" they learn from depend on how the text is split. In agglutinative languages like Turkish and Finnish, thoughtful tokenization = robust representation of morphemes (prefixes, suffixes, infixes, and roots) rather than treating each agglutination as an entirely independent word.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

#### I see. So which ways did you end up tokenizing your text?

TLDR: I used character-level segmentation, character n-grams, word level, and Byte Pair Encoding (BPE) with vocabulary sizes of 5k, 10k, 25k, and 50k. Byte Pair Encoding is a popular tokenization method that merges common pairs of characters until a certain vocabulary size is reached.