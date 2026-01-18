## StegaGen

### Requirements

Installation with Conda is recommended. Tested with Python `3.12`. Run the following to create the environment:

```
conda create -n stega python=3.12
conda activate stega
conda install ipykernel pytorch pytorch-cuda -c nvidia -c pytorch
conda install nltk datasets transformers[torch] tokenizers evaluate sentencepiece huggingface_hub
```