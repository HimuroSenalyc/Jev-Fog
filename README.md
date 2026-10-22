# Jev-Fog

**Beyond MoE.** A self-evolving lightweight LLM architecture tailored for extremely low-VRAM environments.

### Origin
The entire industry is currently trapped in a compute arms race. MoE architectures routinely scale to hundreds of billions of parameters, effectively locking out anyone without a server rack. If you're running on 4GB or 8GB of VRAM, you aren't even allowed on the board.

But let's be honest: the brute-force approach of just amassing parameters has hit a wall. MoE is essentially a "fragmented expert system." It fails miserably on cross-domain tasks, and ironically, it still eats up a ridiculous amount of VRAM because the total parameter count must be loaded.

What we actually need is a singular cognitive entity that "harbors extensive knowledge but activates only a fraction at any given time." Just like human learning—getting sharper with use instead of just getting heavier.

This is where Jev-Fog comes in.

### Architecture Breakdown
This architecture ditches rigid full-scale activation in favor of a paradigm built on **multi-agent debate, fuzzy probing, and hallucination-based integration.**

#### 1. Memory Layer: Scalable Semantic Hash Pool
Instead of maintaining a bloated MoE repository, we establish a 2000B-scale external knowledge base. Knowledge chunks are segmented and stored via semantic hashing.
*Note: Core facts (user profiles, personality parameters) are hard-coded and strictly immutable.*

#### 2. Probe Layer: Jev (Small-Parameter Evaluator)
* **Temperature**: `0.0`
* **Role**: The scout in the fog of war. It obfuscates user input, computes semantic hashes, and retrieves relevant fragments from the massive knowledge base. It doesn't aim for absolute precision—it just illuminates a region in the fog. Jev also acts as the final arbiter for the system's output.

#### 3. Execution Layer: A/B Dual-Model Debate
Both models share the knowledge base extracted by Jev, but their personalities are diametrically opposed.
* **Model A (The Divergent Thinker)**:
 * **Temperature**: `0.9`
 * **Base**: `DeepSeek-R1-Distill-Llama-8B` (CoT length strictly constrained via prompt engineering or max tokens).
 * **Role**: Armed with vague fragments, it goes into extreme hallucination mode, exploring every conceivable connection. It doesn't care about being right; its only job is infinite divergence.
* **Model B (The Convergent Thinker)**:
 * **Temperature**: `0.0`
 * **Base**: `DeepSeek-R1-Distill-Llama-8B`
 * **Role**: A ruthless, cold-blooded code reviewer.
 Zero hallucinations allowed. It clings to logical boundaries, relentlessly nitpicking A's output, forcing A's 360-degree divergence into a precise 10-degree range.

#### 4. Integration Layer: The Chief (Intuitive Stitcher)
* **Temperature**: `0.8`
* **Role**: Operating on minimal context (an extremely compressed hash digest), it receives the aftermath of the A/B debate. If the logic loop is slightly broken, the Chief leverages high-but-sharp hallucinations to stitch the missing pieces together and deliver the final verdict.

#### 5. Evolution Mechanism: Tripartite Cross-Examination
* If the Chief's integration is flawed or uncertain, the output is tagged as `unresolved` and thrown into a temporary buffer.
* **Cross-Examination**: A, B, and the Chief are triggered to review the output simultaneously.
* **Fuzzy Handling**: If the Jev probe yields zero results (or garbage), the system doesn't crash. It forces a generic response using the base model's raw capability, which is then refined through cross-examination.
* High-value thinking paths are recompressed into hashes and written back to the underlying 2000B library. Next time a similar query hits, it's an instant retrieval.

### Core Philosophy
* **Reject MoE**: No fragmented expert committees. A unified core logic absorbing infinite knowledge.
* **Rationalizing Hallucinations**: Hallucinations aren't bugs; they are creative mechanisms that bridge information gaps. By narrowing their focus via dual-model debate and ruthless cross-examination, they ignite the spark of AGI.
* **Forget & Compress**: Replace infinitely expanding KV Cache with semantic hashing and extreme compression.

### Implementation Plan (WIP)
* **Current Stage**: Theoretical framework closed-loop completed.
 1. Rent cloud GPU instances.
 2. Deploy three `R1-Distill-8B` instances on Ollama with distinct temperature configs.
 3. Write the Python middleware: Jev probe routing, A/B debate loops, and the Chief's stitching logic.
 4. Test the "smarter with use" hash self-evolution loop.

> *"If we keep burning compute like this, models will never truly evolve. The real breakthrough lies in forgetting, compression, and strategic interaction."*
