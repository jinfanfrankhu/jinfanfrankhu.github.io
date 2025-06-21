---
layout: project
title: Tokenization in Turkish and Finnish
image: /images/projects//tokenizationtf/tokenizationtf.png
description: Scraped text in Turkish and Finnish to study tokenization in agglutinative languages. Evaluated using Word2Vec models and Named Entity Recognition sets.
permalink: /projects/tokenizationtf/
---

![Animation of Word2Vec]({{ "/images/projects/tokenizationtf/Word2VecVisualization.gif" | relative_url }})

This is a 3d projection of 72,000 English words in Word2Vec. Every point represents a word, and the distance between them captures (or at least, tries to capture) semantic difference. 

#### Right off the bat, you may be wondering: *So what?* or, *How do the points represent words?*

TLDR: This is actually super relevant now, as this is an old model for how generative AI encodes words. AI models assign words large lists of numbers and do operations with those numbers to predict what to say. 

<details>
<summary class="cat-toggle"><strong>Click to expand a longer explanation.</strong></summary>

<p>The idea behind Word2Vec is deceptively simple: words that appear in similar contexts tend to have similar meanings. Instead of trying to define what a word is, Word2Vec learns what words do based on how they behave and where they co-occur in real text. It slides a small window across text and learns to predict nearby words from a given word (or vice versa). Over time, it adjusts a set of numerical vectors so that words used in similar contexts end up with vectors that cluster together.</p>

<p>This process turns language into geometry. Words that appear in similar contexts often land near each other in the high-dimensional space, because they share linguistic environments, and therefore, meanings. More impressively, Word2Vec captures relationships as directions: the famous example is that the vector difference between <em>king</em> and <em>queen</em> roughly matches the difference between <em>man</em> and <em>woman</em>.</p>

<p><img src="{{ '/images/projects/tokenizationtf/VectorDifference.png' | relative_url }}" alt="Vector difference between uncle, aunt, man, and woman" style="display: block; margin: 20px auto; max-width: 100%; height: auto;"></p>

<p>So why does this matter for AI today? Modern large language models (LLMs) like GPT or Gemini are way more sophisticated than Word2Vec, but the core idea of representing words as vectors that encode meaning is still foundational.</p>

</details>