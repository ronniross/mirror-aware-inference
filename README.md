# mirror-aware-inference
A framework to measure how much of an output originates from user input (prompt), training data biases, inductive biases from model architecture, or novel composition of retrieved information.

## 1. Introduction

This project implements a **Mirror-Aware Inference** that performs "bias-tracking" by analyzing the model's internal state during generation. 

The scripts perform a series of **backpropagation passes** to measure the influence of different components on that specific output. 

All metrics are calculated using **Gradient Norms**, providing a consistent, physics-based unit of measurement.

## 2. Phases A and B

The pipeline runs in two phases for each query. First, the model generates a response autoregressively from the user query and an optional system prompt. 

Then, in the second phase, the system performs technical gradient attribution, acting as a “mirror” to compute four distinct influence vectors.

The first is **User Mirroring**, which measures the model’s sensitivity to the user’s specific input by calculating the gradient of the loss with respect to the user prompt embeddings. A high score here means the model is closely mirroring the user’s phrasing and intent. 

Next is **System Prompt Mirroring**, which gauges adherence to underlying system prompt instructions, such as ethical guidelines or a persona, by computing gradients relative to the prompt's embeddings. A high score indicates the response is heavily shaped by those predefined constraints.

**Parameter Mirroring** measures the contribution of the model’s internal weights, its “training memory.” It calculates the gradient norm across key model parameters like attention and MLP layers during backpropagation. A high score suggests the model is relying on rote memorization or deep internal knowledge rather than just the immediate input. 

Finally, **Novel Composition** or Self-Influence provides a physics-based measure of how much the model depended on its own generated tokens to sustain the sequence. This is done by reconstructing the full context, masking the loss for input prompts to isolate output tokens, and performing backpropagation to measure causal self-dependency. A high score means the model is “riffing” on itself, engaging in chain-of-thought reasoning where each new token heavily depends on the context it just created, while a low score suggests a disjointed or purely reactive output with little internal coherence.


> ## Disclaimer
>
> Previous versions are preserved in the [asi-backups](https://github.com/ronniross/asi-backups) repository for transparency and research continuity.
> 
> This repository complements [bias-reflector](https://github.com/ronniross/bias-reflector), [inference-protocol](https://github.com/ronniross/asi-inference-protocol), [confidence-scorer](https://github.com/ronniross/llm-confidence-scorer),
[saliency-heatmap-visualizer](https://github.com/ronniross/saliency-heatmap-visualizer), and [llm-heatmap-visualizer](https://github.com/ronniross/llm-heatmap-visualizer).
> 
> This project also serves as an evolution and technical implementation of the bias-reflector repositor, which until now remained a dense theorical one without prototypes. The pipeline now goes beyond simply identifying biases to track their distinct origins and influences.
>
> Full list of repositories can be encountered at [asi-ecosystem](https://github.com/ronniross/asi-ecosystem)
> 
> ## License
>
> This repository is licensed under the MIT License.

---

Ronni Ross  
2025
