---
layout: project
title: A Neural Network Informed Study on the Gay Voice
image: /images/projects/gayvoice/cover.png
description: An ongoing project with CUNY Queens on identifying linguistic features that correspond with both self-reported sexuality and listener-perceived sexuality in Gen Z youth.
permalink: /projects/gay-voice/
display: true
---

Looking for the survey/gaydar quiz? Click <a href="https://gayvoice.github.io"><b>here</b></a>.

<hr class="project-divider">

#### So what's this project about?

TLDR: We sampled Gen Z males, had them record their voices reading a passage, and asked them to self-rate their sexuality on a 0-10 scale. We're analyzing these recordings using advanced phonetic tools and plan to build neural networks that predict both self-reported sexuality and listener-perceived sexuality.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>This project explores the acoustic features that correlate with both self-reported sexuality and listener perceptions of sexuality in Gen Z male voices. We recruited Gen Z male participants who recorded themselves reading a standardized passage and provided self-ratings of their sexuality on a scale from 0 (100% straight) to 10 (100% gay), with 5 representing 50-50 bisexual.</p>

<p>The acoustic analysis pipeline involves multiple sophisticated tools: we use Praat for phrase-level phonetic annotations, the Montreal Forced Aligner for precise phoneme-level segmentation, and Santiago Barrera's FastTrack repository for formant tracking. This multi-layered approach allows us to extract detailed acoustic features ranging from fundamental frequency patterns to spectral characteristics of specific phonemes like sibilants.</p>

<p>The project is currently in the analysis phase, with plans to extend into perceptual studies where listeners will rate how "gay" the voices sound. This will give us two distinct prediction targets: self-reported sexuality scores and listener-perceived sexuality scores.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

#### What makes this approach unique?

TLDR: We're using a two-stage neural network approach: first, interpretable models using hand-crafted phonetic features, then high-capacity models on raw audio. This bridges the gap between linguistic theory and modern AI methods.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p><strong>Stage 1: Interpretable Models (Feature → Prediction)</strong><br>
We start with hand-crafted sociophonetic features including fundamental frequency (F0), formant frequencies, silibant center of gravity, rhythmic patterns, and pausing behavior. These features are fed into transparent models like logistic regression, random forests, or linear SVMs. Using feature importance tools (coefficients, SHAP values, permutation importance), we can identify which acoustic properties are significant predictors of sexuality perception.</p>

<p>This approach may not achieve state-of-the-art accuracy, but it provides crucial evidence that aligns with linguistic research expectations: "Pitch range and /s/ spectral characteristics are significant predictors of perceived sexuality."</p>

<p><strong>Stage 2: High-Capacity Predictive Models</strong><br>
We then train deep neural networks on raw waveforms or self-supervised learning embeddings (Wav2Vec2, HuBERT, Whisper) to maximize predictive accuracy on held-out speakers and listeners. To understand what these black-box models are learning, we employ several probing techniques:</p>

<p><strong>Stage 1.5: Targeted Feature Ablation</strong><br>
Once we identify which interpretable features matter most in Stage 1, we systematically remove them from the neural network's input. For example, if /s/ center-of-gravity is important in our interpretable models, we mask sibilant regions in the raw audio for Stage 2 models to see how prediction accuracy changes.</p>

<p>This bridging approach gives us both interpretable linguistic insights and high-performance predictive models, allowing us to say: "A black-box model predicts perceived sexuality at X% accuracy, and when we probe it, we find it relies heavily on prosodic features and sibilants."</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

#### What tools and methods are you using for acoustic analysis?

TLDR: Praat for phonetic annotation, Montreal Forced Aligner for phoneme alignment, and FastTrack for formant analysis. (Possibly more, will update.) This gives us both broad prosodic patterns and fine-grained segmental details.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p><strong>Praat Phrase-Level Annotations</strong><br>
Praat is the gold standard for phonetic analysis in linguistics research. We use it to extract prosodic features like pitch contours, intensity patterns, and rhythm metrics across entire utterances. This captures the suprasegmental aspects of speech that often carry social meaning. We hand-made phrase-level annotations to feed into the Montreal Forced Aligner.</p>

<p><strong>Montreal Forced Aligner (MFA)</strong><br>
MFA provides precise time alignments between the audio and phoneme sequences, allowing us to analyze specific sound segments. This is crucial for studying features like /s/ spectral characteristics, vowel formant patterns, and consonant-vowel timing relationships that have been implicated in previous research on sexuality perception. The phoneme-aligned textgrids are then fed into FastTrack.</p>

<p><strong>FastTrack Formant Analysis</strong><br>
Professor Santiago Barrera's FastTrack repository offers robust formant tracking across different speakers and recording conditions. Formant patterns (especially F1 and F2) are key indicators of vowel quality and have been linked to perceived speaker characteristics in sociolinguistic research.</p>

<p>This multi-tool approach ensures we capture acoustic variation at multiple timescales and levels of detail, from millisecond-level phoneme characteristics to sentence-level prosodic patterns.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

#### What's next for this project?

TLDR: We're currently analyzing the acoustic data and preparing to launch listener perception studies. The goal is to train two separate neural networks - one for self-reported scores and one for listener-rated scores - to understand the difference between identity and perception. If you'd like to test your gaydar, please do so by taking the survey <a href="https://gayvoice.github.io" target="_blank" rel="noopener noreferrer">here</a>.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p><strong>Current Phase: Acoustic Analysis</strong><br>
We're extracting comprehensive acoustic features from the recorded speech samples using our multi-tool pipeline. This includes fundamental frequency statistics, formant trajectories, spectral characteristics of fricatives and sibilants, rhythm and timing metrics, and voice quality measures.</p>

<p><strong>Upcoming: Listener Perception Studies</strong><br>
We plan to have listeners rate how "gay" the voices sound, giving us a second prediction target alongside the self-reported sexuality scores. This will allow us to explore the potentially important distinction between sexual identity and how sexuality is perceived by others.</p>

<p><strong>Dual Neural Network Approach</strong><br>
We'll train two separate neural network models: one trained on self-identified sexuality scores and another trained on listener-rated scores. Comparing these models will reveal whether the acoustic cues that predict self-identity are the same as those that drive listener perceptions, or if there are systematic differences.</p>

<p>This approach addresses a key question in sociolinguistics: Do people who identify as gay use different acoustic features than what listeners actually perceive as "sounding gay"? The answer has important implications for understanding how sexual identity is both performed and perceived through speech.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

