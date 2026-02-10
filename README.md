## StegaGen

Perform steganography by fine-tuning pre-trained LLMs to hide secret bits in token probabilities. A corresponding decoder network can retrieve these secret bits again from the encoded message.

Architecture:
- Any causal LM as a generation base (Tested with SmolLM2-360M-Instruct, Qwen3-0.6B and TinyLlama-1.1B-Chat)
- A learned vocabulary mask that masks token probabilities based on secret bits
- A decoder to retrieve secret bits

### Training

Training is driven by encoder/decoder performance and regulated by KL semantic loss against a frozen copy of the base language model. A learning schedule is implemented to weight secret recovery performance more heavily during initial training, and shift to semantic preservation once the communication channel between encoder and decoder is established. 

### Results

After 100 training steps, the following secret recovery accuracy was achieved on the three tested models:

![Accuracy](results/accuracy.png)

Both the SmolLM-360M and Qwen3-0.6B perform very well with our implementation, with Qwen3 producing slightly more natural looking output text (despite possessing worse semantic loss metrics). These results allow for effective steganographic communication, especially when combined with traditional redundancy and error-correcting techniques. 

Training graphs for secret recovery loss ($l_{rec}$) and semantic loss ($l_{sem}$) are shown below.

![Recovery loss](results/l_rec.png)
![Semantic loss](results/l_sem.png)


### Example

The trained model can be used with the `encode()` and `decode()` APIs. Below is an end-to-end example:
```python
# Generate and decode stega text
carrier = 'Write a two-line poem about neural networks.'
secret = generate_secret()

stego_text = encode(carrier, secret)
recovered_secret = decode(stego_text)

print('Prompt:           ', carrier)

print('Stego text:       ', stego_text)
print()
print('Original secret:  ', bits_to_str(secret[0]))
print('Recovered secret: ', bits_to_str(recovered_secret))
print(f'Accuracy:          {100 * calculate_accuracy(recovered_secret, secret):.2f}%')
```

Output:
```
Prompt:            Write a two-line poem about neural networks.
Stego text:        In the maze where wisdom is tangled through neural wires, Each spark leads to a new path, A world of patterns where logic and light meet.

Original secret:   0011111110011011
Recovered secret:  0011111110011011
Accuracy:          100.00%
```

### Requirements

Installation with Conda is recommended. Tested with Python `3.12`. Run the following to create the environment:

```
conda create -n stega python=3.12
conda activate stega
conda install ipykernel pytorch pytorch-cuda nltk datasets transformers[torch] tokenizers huggingface_hub pandas matplotlib -c nvidia -c pytorch
```