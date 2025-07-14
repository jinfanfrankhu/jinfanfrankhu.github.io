---
layout: project
title: Generating Language Families using Word Similarity
image: /images/projects/tuftsudhr/cover.png
description: "2024 Summer Project for Tufts Engineering with AI Camp: Using the UDHR, ran a comparison using Levenshtein Distance to \"classify\" language families."
permalink: /projects/tuftsudhr/
---

For this project, I tried to explore reverse-engineering language families according to the similarity of the words. This was done by using <a href="https://en.wikipedia.org/wiki/Levenshtein_distance" target="_blank">Levebshtein distance</a>.

<p><img src="{{ 'images/projects/tuftsudhr/indoeuropeanfamilies.jpeg' | relative_url}}" alt="An illustrated rendering of the Proto-Indo-European language family" style="display: block; margin: 20px auto; max-width: 100%; height: auto;"></p>

#### So uh... what is a language family?

TLDR: Language families are groups of languages that are related through a common ancestor (a language that was spoken before, that diverged to create new languages). Some examples include Romance Languages, Germanic Languages, and Semitic Languages.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>The image above is an artist's rendering of the <i>Indo-European</i> language family. As you can see, it includes many of the world's most spoken languages such as English, Spanish, Hindi, Bengali, French, Russian, and Persian. Yup, all of these languages had the same common ancestor, which is why they're all classified under one family/tree.</p>

<p>For Indo-European, the big leap happened in 1786, when Sir William Jones was looking at Sanskrit (an ancestor of Hindi) and comparing them to other classical European languages, where he discovered that a lot of vocabulary was similar. For example:</p> 

<table style="display: block;">
  <tr>
    <th>English</th>
    <th>Latin</th>
    <th>Sanskrit</th>
  </tr>
  <tr>
    <th>donation</th>
    <th>dōnum</th>
    <th>दान (dāna)</th>
  </tr>
  <tr>
    <th>new</th>
    <th>novus</th>
    <th>नव (nava)</th>
  </tr>
  <tr>
    <th>interior</th>
    <th>intra</th>
    <th>अन्तर (antara)</th>
  </tr>
  <tr>
    <th>mother</th>
    <th>māter</th>
    <th>मातृ (mātr)</th>
  </tr>
<table>

<p>Of course, determining language families is not so easy as just comparing words. Taking English as an example, we took tons of vocabulary from the <a href="https://en.wikipedia.org/wiki/Influence_of_French_on_English" target="_blank">French</a>, but we're actually a Germanic family language. You can take a looksie at what English would've looked like without French influence: it's called <a href="https://anglish.org/wiki/Anglish" target="_blank">Anglish</a>! 

<p>Unfortunately, it's hard to classify language families at times. Theories come and go on language families, and they're always complicated by actual history. For the case of English, the reason we had so much French influence was because of the Norman Conquest of England. An example of where trying to use history to back-track language spreading failed is the <a href="https://en.wikipedia.org/wiki/Altaic_languages" target="_blank">Altaic language family</a>, a theory that was supported by the nomadic migration of the peoples in the areas that "Altaic" languages were spoken. Unfortunately, the similarities that the languages shared were largely coincidental, and Altaic is no longer supported.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

#### So you said that determining language families aren't done by just comparing words. Levenshtein distance is just comparing words... no?

TLDR: Woah, shots fired, you got me there! I was just messing around with words to try and make families of lexical similarity! You're right that Levenshtein distance looks at words, and there's a lot of limitations with what I was doing! Guess a five-day project can't be too deep after all.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>So to start off, a quick review of what <a href="https://en.wikipedia.org/wiki/Levenshtein_distance" target="_blank">Levenshtein distance</a> is! </p>

<img src="{{ '/images/projects/tuftsudhr/levenshteinformula.png' | relative_url }}" alt="A formula for deriving Levenshtein Distance" style="display: block; margin: 20px auto; max-width: 100%; height: auto;">

<p>Yeah, I know. Don't worry though, despite this math looking really complicated, it's actually pretty simple. Levenshtein distance is basically a measurement of <i>how many letters you have to substitute, add, or delete to get from word a to word b.</i> An example: the Levenshtein distance between the words "Frank" and "Tank" is two! Substitute "r" for "T" (+1 to distance), and delete "F" from "FTank" (+1 to distance). This is often used in spell-check algorithms: if a word isn't recognized in the spell-checker's dictionary, it'll ask you if you meant words that are a small Levenshtein distance away. It's also used in... drumroll please... comparative linguistics! Because languages with a shared common ancestor should have tons of cognates with each other, levenshtein distance becomes a great way to measure the linguistic distance between two languages!</p>

<img src="{{ '/images/projects/tuftsudhr/dutchmeme.png' | relative_url }}" alt="A meme of Dutch's similarity with English" style="display: block; margin: 20px auto; max-width: 100%; height:auto;">

<p>I mean... come on! Isn't that hilarious? Dutch is so similar to English! And best of all, we can look at the Levenshtein distance to try and quantify that distance. We hebben een serieus probleem -- We have a serious problem! The Levenshtein distance between these two sentences is tiny: 0 from we to we, 4 for hebben to have, 3 from een to a, 1 from serieus to serious, 1 from probleem to problem. (If you're curious, the rest translates to: "with the political developments regarding the coercion law, and I hope that this can be resolved in the coming days." Not as similar as we hebben een serieus probleem lol!) Anyways, using this, I tried to compare all sorts of different languages to each other.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>