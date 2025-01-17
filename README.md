# audio-visual-reranking

This repository builds from the data and models created [here](https://github.com/Cylumn/embodied-multimodal-asr/). 

This repository evaluates the utility of pre-trained language and vision models in re-ranking hypotheses generated by a speech recognition model, given visual context of the listener at the time of speaking. [CLIP](https://github.com/openai/CLIP) and [GPT-2](https://github.com/openai/gpt-2) are used to generate a log-probability score for the likelihood of a transcript given the visual context and the likelihood of the transcript given GPT-2's training data respectively. A shallow neural network is trained discriminatevely to identify the lowest-WER hypothesis from the top-k hypotheses (5, here) given acoustic, visual, and linguistic likelihood indicators of each hypothesis. For the initialization provided in this repository with a unimodal base model, a 10% relative improvement in WER is observed.

This repository uses the [ALFRED](https://askforalfred.com/) dataset and accompanying synthetic spoken instructions generated [here](https://github.com/Cylumn/embodied-multimodal-asr/). 

## Setup

After cloning the repository, install the requirements:
> $ pip install -r requirements.txt

Download the dataset:
> $ sh download_data.sh full

Generate synthetic spoken instructions:
> $ python preprocess.py

## Training

To train the reranker, run the following command:
> $ python train.py --run='{base}_[{noise}]_50' --id_noise='{noise}' --dir_source='data'

**base**: 'unimodal' or 'multi[clip]': whether baseline model to generate transcripts is unimodal (only uses audio) or multimodal (uses audio & image)

**noise**: 'mix_clean' or 'mix_mask_1.0_nouns' or 'mix_mask_0.4_all': noise setting used to train base model and reranker; of the form (speaker-type)\_(noise-type)\_(proportion-of-tgt-masked)\_(target-words)
