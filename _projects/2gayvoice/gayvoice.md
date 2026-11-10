---
layout: project
title: Can a Computer Tell If You're Gay From Your Voice?
image: /images/projects/gayvoice/cover.png
description: With CUNY Queens College | Accepted—Lavender Languages 32 | Accepted—NWAV 54
permalink: /projects/gay-voice/
display: true
date: 2026-04-09
---

Can you really tell someone's sexuality just from their voice? What does it mean to sound gay?

To answer this question, I had 50 male-identifying Gen Z people read [this passage](/projects/gay-voice/passage/) while I recorded them. Then, I asked them to rate their attraction on the following scale:

1: Only attracted to women
2: Mostly attracted to women
3: No preference
4: Mostly attracted to men
5: Only attracted to men

I also had people listen to the audio files and give me a best guess as to what they thought the speaker's sexuality is, on the same scale.

Here's a plot of how well people guessed, with each point in the plot being a different speaker:

![Perceived score vs actual Kinsey score for each of the 50 speakers]({{ "/images/projects/gayvoice/f25/perceived_vs_actual_lsrl.png" | relative_url }})

As you can see, people aren't too good at guessing, but there was some statistical correlation. 

Now the interesting question: can computers do any better? There are a couple ways you can go about doing this. First, I tried extracting features from the audio files using <a href="https://audeering.github.io/opensmile/" target="_blank" rel=noreferrer noopener>openSMILE</a>'s <a href="https://sail.usc.edu/publications/files/eyben-preprinttaffc-2015.pdf" target="_blank" rel=noreferrer noopener>eGeMAPS</a>, which is a set of 88 linguistically relevant features such as pitch variation, jitter, loudness, etc. I also used something called the <a href="https://montreal-forced-aligner.readthedocs.io/en/latest/" target="_blank" rel=noreferrer noopener>Montreal Forced Aligner</a> to extract stuff about actual phonemes (the /s/ sound, the /z/ sound, how people pronounce the vowel /a/ vs /e/, etc.)

Side note: going forward, I'm going to be referring to the average score that listeners guessed a file to be as the "perceived score," and I'm going to be referring to the actual score that the speaker gave me as the "actual score."

According to some statistical testing, (Benjamini-Hochberg FDR, if you're curious), when it came to actual score, not a single feature was significant when it came to determining sexuality. It's almost as if the way you talk isn't an indicator of sexuality! Wow!

For perceived score, 10 features passed. In order from strongest to weakest association: "AY_z2", "S_cog_singleton", "S_peak", "S_cog", "Z_peak", "Z_cog", "AE_z2", "AO_z1", "EY_z2", and "AW_z1". Basically, the way people that sound gay pronounce their /s/, /z/, and vowels may be what people are picking up on. Let's keep that in the back of our minds as we go to the other method I used. 

Machine learning time. I used <a href="https://arxiv.org/abs/2110.13900" target="_blank" rel=noreferrer noopener>WavLM</a>, a library developed by Windows, to basically get a numerical representation of "frames" (~40ms chunks) of audio. Then, I trained lightweight ridge and elastic-net models on top, which learned very quickly how to predict perceived score. As expected, it didn't do any better than people did on predicting actual gayness from speech. What's nice about WavLM is that I get to peek at which frames the model looks at / weights more heavily when deciding the predicted score. Awesome! Let's see if it matches up with our predictions from the statistical stuff we did before...

![Which phone classes each model leans on, ridge vs elastic net]({{ "/images/projects/gayvoice/f25/saliency_phones.png" | relative_url }})

...and it doesn't. On the left, the ridge model (blue) says vowels are what make you sound gay and that /s/ actually pushes you toward sounding straight. The elastic-net model (orange) mostly says the opposite. Same 50 speakers, same audio, same WavLM numbers underneath, and the two models predict perceived score about equally well. They just completely disagree on why. This has a name: the <a href="https://en.wikipedia.org/wiki/Rashomon_effect" target="_blank" rel=noreferrer noopener>Rashomon effect</a>. In machine learning, it's when lots of different models fit the data equally well but tell different stories about what matters. The set of all those equally good models is called the "Rashomon set." WavLM turns each speaker into a set of 768 numbers, and I only have 50 speakers. It's basically trying to solve for 768 unknowns with 50 equations. 

In conclusion: listeners agree on what a "gay voice" sounds like, but that agreement only loosely tracks who is actually gay (ρ = 0.55). No acoustic feature predicts actual score. Perceived score is tied to /s/, /z/, and a few vowels. WavLM predicts perceived score better than hand-measured features (ρ ≈ 0.73), but still can't predict actual score. Because of the Rashomon effect, asking one model what it listens to isn't reliable. Once I compared all the equally good models and accounted for vowels simply being more common, /s/ came out on top across model types and across three different speech models. So "gay voice" seems to be a shared stereotype built mostly on /s/, not a real signal of sexuality.

This work was presented at New Ways of Analyzing Variation (NWAV54) hosted at the University of Montreal, and Lavender Linguistics 32 hosted at the University of Edinburgh.

Poster can be found here (<a href="{{ "/images/projects/gayvoice/NWAV54_poster.pdf" | relative_url }}" target="_blank" rel=noreferrer noopener>NWAV54 poster, PDF</a>)

Code: https://github.com/jinfanfrankhu/perceived-gay-voice-study/tree/main/F25
Dataplots: https://github.com/jinfanfrankhu/perceived-gay-voice-study/tree/main/F25/outputs

![Frank with his poster at NWAV54, Université de Montréal]({{ "/images/projects/gayvoice/nwav54_poster_photo.jpg" | relative_url }})
