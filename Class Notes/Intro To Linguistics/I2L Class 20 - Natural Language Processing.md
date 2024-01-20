---
tags:
  - intro-to-linguistics
---
Computational Linguistics is the general approach of thinking about everything in linguistics as computation and/or utilizing computational methods to address linguistic research questions

NLP introduces engineering to the picture and attempts to approximate aspects of human linguistic functioning in non-humans

Computational models isnpired by biological neural systems

Artificial neural networks have input layers, hidden layers and output layers

ANNs work through training, usually done using supervised learning (give inputs, tell what it should output)

If there are no labels and the network tries to just identify patterns and similarities, thats unsupervised learning

Or if you use a reward signal, that’s reinforcement learning

speech synthesis tries to sound like a human and is judged in terms of the intelligibility and naturalness

Concatenative speech synthesis records actual human speech and recombines it in novel ways

The most effective method is to use diphones (two sounds together) this accounts for coarticulation effects and makes more natural speech

Prosodic information can be stored separately and speech sounds can be modified accordingly to develop human like prosody

State of the art in speech synthesis use convolutional neural networks. Model is fed phonetic and linguistic information (phoneme, syllable, word) → applicable in TTS

Automatic Speech Recognition aims to recognize what people say

seems easy, but noise and variability between speakers complicates inputs

First need to process signals (store auditory input digitally), then need to map energy levels to some stored/trained set of energy levels of sounds Then need to use stored/trained knowledge, acoustic info to separate words and map the combo of sounds onto likely words, and then recognize words and their meanings

Domain specific ASR is easy, domain general is difficult

Relies on context, co-occurence frequencies and combines bottom up and top down processing

A corpus is a collection of text or recordings

Corpora have a wide range of uses (calculating frequencies, training machine translation, training chat bots)

word- and phrase- frequencies are different in different genres so source matters

Depending on purpose of the corpus, different aspect of the text will be annotated (intonation, part of speech, morpheme, syntactic, speech act, sociolinguistic, etc.)

Annotations can be hand or machine coded

Machine translation is catching up to humans

If a human knows two languages fluently, the ycan translate better than any machine, especially in regard to pragmatics and social components of language

But machines can translate 100s of languages

Dictionary approach: just look up the translation

In domain specific applications, works well

Struggles with lexical ambiguity, full sentences, pragmatic, idioms, etc.

Transfer-based and interlingua-based machine translation uses some sort of intermediary representation between the source language and the target language

Transfer based converts source lang to source lang related intermediary to target lang related intermdiary to target lang

Interlingua goes from SL to a lang independent intermediary to TL

SL is analyzed and parts are identified, converted to the intermediaries, converted to TL

Morphological and syntactic analysis of SL

Lexical categorization and transfer (word → concept/semantic features → word)

syntactic and morphological generation in TL

think about how hierarchical structure and syntax will help in this endeavor

Deeper the structure obtained, “farther” a translation will be possible

Transfer based is best for two closely related languages that share higher degree of cognates, syntactic structure, and typology. Interlingua based handles unrelated languages well

Example-based machine translation uses bilingual corpora

Decompose the input into phrases, find translations of those phrases, reconstruct the translation

Statistical machine translation gathers statistics from corpora and applied learned probabilities to novel translation

Neural machine translation is the state of the art relying on deep learning

Human-Computer Communication

Essentially a culmination of all others: must receive input, understand meaning, develop responses, output responses intelligibly as natural as possible

The less general the domain, the better the performance will be

Turing test isnt really a test nor was it intended to be, but essentially it test the degree to which a machine can fool a person into believing that they are communicating with a live person

Winograd Schemas - sentences with ambiguous references expounded upon later in the sentence
