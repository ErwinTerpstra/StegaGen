## StegaGen

Perform steganography by fine-tuning pre-trained LLMs to hide secret bits in token probabilities. A corresponding decoder network can retrieve these secret bits again from the encoded message.

Architecture:
- Any causal LM as a generation base (Tested with SmolLM2 135M)
- A learned projection of secret bits onto token probabilities
- A decoder MLP network to retrieve secret bits

### Training

Training is driven by encoder/decoder performance and regulated by KL semantic loss against a frozen copy of the base language model. A learning schedule is implemented to weight secret recovery performance more heavily during initial training, and shift to semantic preservation once the communication channel between encoder and decoder is established. 

### Requirements

Installation with Conda is recommended. Tested with Python `3.12`. Run the following to create the environment:

```
conda create -n stega python=3.12
conda activate stega
conda install ipykernel pytorch pytorch-cuda nltk datasets transformers[torch] tokenizers evaluate sentencepiece huggingface_hub -c nvidia -c pytorch
```