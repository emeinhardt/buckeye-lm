# buckeye-lm

This repository contains two notebooks for working with the Buckeye corpus. In particular, the code here
 - aligns transcriptions with Unicode IPA symbols.
 - produces representations of the Buckeye corpus data convenient for use with a language model (specifically one trained on the Fisher corpus).

## IPA alignment

The goal of the notebook `Converting Buckeye Transcriptions to Unicode IPA symbols` is to develop/document code 
 - to be able to access Buckeye data where all segment symbols have been converted to use IPA representations.
 - to identify and fix (some) annotation anomalies in the Buckeye data.

**Note!** The alignment and 'anomaly' fixes documented here are based on
 - what limited documentation I have found about the process of corpus generation.
 - my intended use cases.

Additional documentation, more intense scrutiny of the data (e.g. of spectrograms), and different applications could very plausibly motivate different decisions. *The point* of this notebook is to document my exploration of the corpus and my alignment/anomaly patch decisions.

### Dependencies

Critical:
 - Scott Seyfarth's wonderful `buckeye` package (see https://github.com/scjs/buckeye)
 - your own local copy of the Buckeye corpus
 
Convenient, but non-essential and relatively easy to replace:
 - `more_itertools`
 - `funcy`
 
For some analysis/plotting; convenient, but non-essential and relatively easy to replace with your preferred libraries:
 - `pandas`
 
 ### Results / outputs?
 
 Given the size of the Buckeye data (it's a 2.5GB corpus - not just a lexicon in a single `.csv` file) and the fact that there's already a nice existing Python package for interfacing and interacting with the corpus, I'm not going to make an IPA'd *copy* of the Buckeye data, just some basic functions for converting representations. 

*Code at the end of the notebook = the main outcome of this notebook.*

## Language-model friendly and relational representations of corpus data

The goal of the notebook `Preprocessing Buckeye corpus transcriptions for ease of processing and use with kenlm.ipynb` is to produce (/document the production of) a representation of Buckeye corpus data whose vocabulary has been normalized with respect to the Fisher corpus and where utterance segmentation has been performed. The motivation for doing this is applying a language model trained on (a slightly processed version of) the Fisher corpus to Buckeye.

### Processing steps

To that end, 
 1. A smattering of orthographic wordforms and transcriptions are aligned and/or corrected per the other notebook in this repository (`Converting Buckeye Transcriptions to Unicode IPA symbols`).
 2. Utterances are segmented per Seyfarth (2014) / his `buckeye` package.
 3. Non-speech noises (e.g. `[laughter]` or `[silence]`) are removed from utterances.
 4. All orthographic characters are lower-cased.
 
### Dependencies

Critical:
 - A local copy of the Buckeye corpus data.
 - The `buckeye` python package.
 
Less important: 
 - `funcy`
 - `pandas`
 - `plotnine`
 
 ### Outputs
 
 If run successfully, this notebook will create four files as outputs:
 1. A .json file containing a list of objects (Python dictionaries), where each object is a finitary relation describing an utterance (and associated metadata) in the Buckeye corpus.
 2. A .txt file containing one utterance from Buckeye per line, suitable for use with a language model.
 3. A .txt file containing the vocabulary (one wordform per line) of the previous file.
 4. A .json file containing a list of objects (Python dictionaries), where each object is a finitary relation describing a wordform token (and associated metadata) in the Buckeye corpus.
