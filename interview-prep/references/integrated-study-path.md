# Integrated Study Path: Algorithms × LLMs × Linguistics

Three courses, one path. Every exercise hits all three domains simultaneously.

## The Three Pillars

| Pillar | Source | Maps to (Prior Learning Credit) | Credits |
|--------|--------|--------------------------------|---------|
| **Algorithms & Data Structures** | Sedgewick & Wayne, *Algorithms 4th Ed* (algs4.cs.princeton.edu) | COS 226 / CS 201: Data Structures & Algorithms | 3-4 |
| **LLM Fundamentals** | Raschka, *Build a Large Language Model from Scratch* | CS 4xx: Intro to Machine Learning / NLP | 3 |
| **Linguistics** | MIT 24.900 (OCW) + historical English corpora | LING 101: Intro to Linguistics | 3-4 |
| **Integrated Project** | Notebooks, blog posts, corpus analysis, portfolio | Independent Study / Capstone | 3 |

**Potential total: 12-14 credits at SUNY Empire** via prior learning assessment.

## How They Weave Together

```
Sedgewick (DSA)          Raschka (LLMs)           MIT 24.900 (Ling)
     |                        |                        |
  Strings ──────────── BPE Tokenization ─────── Morphology
  Tries                 Vocab building           Morpheme segmentation
  Compression           Merge sequences          Morpheme economy
     |                        |                        |
  Graphs ───────────── Attention Mechanisms ──── Syntax
  BFS/DFS               Self-attention           Constituency/dependency
  Shortest paths         Positional encoding      Word order typology
     |                        |                        |
  Searching ────────── Embeddings ───────────── Phonology
  Hash tables            Token lookup             Feature matrices
  Symbol tables          Vocabulary               Phonemic inventory
     |                        |                        |
  Sorting ──────────── Classification ──────── Dialects/Register
  Priority queues        Top-k sampling           Sociolinguistic variation
  Ranking                Beam search              Prestige hierarchies
     |                        |                        |
  Fundamentals ─────── Pretraining ──────────── Phonotactics
  Analysis (Big-O)       Why attention is O(n²)   Distributional constraints
  Stacks/Queues          Residual connections      Sequential processing
     |                        |                        |
  Context ──────────── Fine-tuning/RLHF ─────── Semantics/Pragmatics
  Reductions             Instruction following    Speech acts (Austin/Searle)
  Intractability         Why exact inference hard  Cooperative Principle (Grice)
```

## The Historical English Thread

Every unit includes exercises on historical English texts that hit all three pillars:

| Period | Example Text | Why It's Useful |
|--------|-------------|----------------|
| **Old English** (450-1100) | Beowulf, Anglo-Saxon Chronicle | Rich inflectional morphology, free word order (SOV/V2), runic orthography. BPE merges reveal case endings. Graph algorithms on syntactic dependency. |
| **Middle English** (1100-1500) | Canterbury Tales (Chaucer) | Transitional — losing inflections, gaining fixed word order. French loanword layer. BPE shows bilingual merge dynamics. |
| **Early Modern English** (1500-1700) | Shakespeare, King James Bible | Great Vowel Shift in progress. Thou/you distinction dying. Regularization visible in BPE patterns. |
| **Contemporary English** (1700-now) | NYT, academic prose, Reddit | Minimal morphology, strict SVO, analytical grammar. BPE baseline. |

The progression shows English losing synthetic morphology and gaining analytic structure — and BPE's merge behavior tracks this shift computationally.

---

## Unit Schedule (24 weeks / 6 months)

### Unit 1: Foundations (Weeks 1-4)

**Theme**: What are the building blocks?

| Week | Sedgewick | Raschka | MIT 24.900 | Integration Exercise |
|------|-----------|---------|------------|---------------------|
| 1 | 1.1-1.2: Programming model, data abstraction | Ch 1: Understanding LLMs (conceptual) | L1: Introduction to linguistics | **Notebook 1**: Set up environment. Write a "what is language?" essay that connects linguistic structure to computational structure. Define terms across all three domains. |
| 2 | 1.3: Stacks and Queues | Ch 2 (start): Text preprocessing, tokenization basics | L2: Morphology (Part 1) — morphemes, affixes | **Notebook 2**: Implement a stack-based tokenizer. Segment words from Beowulf into morphemes manually. Compare your segmentation with a naive whitespace tokenizer. |
| 3 | 1.4: Analysis of Algorithms (Big-O) | Ch 2 (cont): BPE algorithm | L3: Morphology (Part 2) — derivation, compounding | **Notebook 3**: Implement BPE from scratch. Analyze its time complexity (Big-O). Run on OE/ME/EModE/Contemporary English. Blog post: "BPE Discovers Morphology" — show merge sequences across historical periods. |
| 4 | 1.5: Union-Find | Ch 2 (finish): DataLoader, embeddings intro | L4: Morphology (Part 3) — morphological typology | **Notebook 4**: Use Union-Find to cluster BPE tokens by morphological family. Compare English (analytic) vs. Icelandic (synthetic) tokenization. What does BPE's merge tree tell us about morphological typology? |

**Milestone**: Can implement BPE, analyze its complexity, and explain morphological parallels.
**Blog post**: "BPE Discovers Morphology: What Tokenizer Merge Sequences Reveal About English's Lost Case System"
**Filmed walkthrough**: Explain BPE + morphology mapping on camera (Stage 1 performance ramp).

---

### Unit 2: Features and Patterns (Weeks 5-8)

**Theme**: How do we represent and find patterns?

| Week | Sedgewick | Raschka | MIT 24.900 | Integration Exercise |
|------|-----------|---------|------------|---------------------|
| 5 | 3.1: Symbol Tables | Ch 2 (review): Token embeddings | L5: Phonetics (Part 1) — IPA, articulatory features | **Notebook 5**: Build a symbol table mapping tokens to embeddings. Compare with phonetic feature matrices (±voice, ±nasal, ±continuant). Both are distributed representations. |
| 6 | 3.4: Hash Tables | Ch 2 (deep): Embedding lookup, vocab hashing | L6: Phonetics (Part 2) — acoustic phonetics | **Notebook 6**: Implement hash-based vocabulary lookup. Analyze collision behavior with OE vocabulary (lots of similar-looking inflected forms) vs. ModE. Why does morphological complexity affect hash distribution? |
| 7 | 3.2: Binary Search Trees | Ch 3 (start): Simple attention | L7: Phonetics (Part 3) — transcription practice | **Notebook 7**: BST for sorted vocabulary access. Begin attention mechanism. Phonetic transcription of historical English passages — track which sounds the model struggles with. |
| 8 | 3.3: Balanced Search Trees (Red-Black) | Ch 3 (cont): Scaled dot-product attention | L8: Phonology (Part 1) — phonemes, allophones | **Notebook 8**: Red-black trees for balanced lookup. Attention as "which context matters" — phonological context sensitivity (allophones depend on neighbors, like attention depends on surrounding tokens). |

**Milestone**: Understand hashing, trees, embeddings, and attention fundamentals.
**Blog post**: "Embeddings Are Feature Matrices: What Phonetics Taught Me About Distributed Representations"
**DSA practice**: Neetcode arrays/hashing/trees problems alongside (per anthropic-ramp schedule).

---

### Unit 3: Structure and Relations (Weeks 9-12)

**Theme**: How do parts compose into wholes?

| Week | Sedgewick | Raschka | MIT 24.900 | Integration Exercise |
|------|-----------|---------|------------|---------------------|
| 9 | 3.5: Searching applications | Ch 3 (cont): Multi-head attention | L9: Phonology (Part 2) — phonological rules | **Notebook 9**: Searching applications (frequency counts, concordances). Each attention head as a "rule" detector. Phonological rules as context-sensitive rewriting — compare with attention patterns. |
| 10 | 4.1: Undirected Graphs, DFS/BFS | Ch 3 (finish): Causal attention, positional encoding | L10: Phonology (Part 3) — rule ordering, opacity | **Notebook 10**: Graph representations. Positional encoding as "where am I in the sentence?" OE word order freedom vs. ModE rigidity — visualize attention patterns on both. Why does positional encoding matter more when word order is fixed? |
| 11 | 4.2: Directed Graphs, topological sort | Ch 4 (start): GPT architecture, layer norms | L11-12: Syntax (Part 1-2) — constituency, phrase structure | **Notebook 11**: Directed graphs and parse trees. Transformer layers as composition — phrase structure rules compose meaning, transformer layers compose representations. Parse OE vs. ModE sentences. |
| 12 | 4.3-4.4: MSTs, Shortest Paths (Dijkstra) | Ch 4 (finish): Full GPT implementation | L13-14: Syntax (Part 3-4) — transformations, movement | **Notebook 12**: Shortest paths — connect to PPR in qortex (you already know this!). Full GPT implementation. Syntactic movement as "long-distance dependency" — attention can model this directly. |

**Milestone**: Full GPT implemented. Graphs understood. Syntax-attention parallel articulated.
**Blog post**: "Attention Is (Not) All You Need: What Syntax Tells Us About Transformer Architecture"
**Connection to qortex**: Dijkstra/BFS → PPR. This is where DSA meets your actual work.

---

### Unit 4: Learning and Prediction (Weeks 13-16)

**Theme**: How do systems learn from data?

| Week | Sedgewick | Raschka | MIT 24.900 | Integration Exercise |
|------|-----------|---------|------------|---------------------|
| 13 | 5.1-5.2: String Sorts, Tries | Ch 5 (start): Pretraining, loss functions | L15-16: Syntax (Part 5-6) — binding, control | **Notebook 13**: Tries for efficient vocabulary operations. Pretraining as "learning distributional constraints." Long-distance syntactic dependencies (binding theory) as motivation for why attention needs full context. |
| 14 | 5.3: Substring Search (KMP, Boyer-Moore) | Ch 5 (cont): Training loop, gradient descent | L17: Syntax (Part 7) + Semantics (Part 1) | **Notebook 14**: Substring search algorithms. Training dynamics visualization. Semantics begins — meaning composition. How does the model's loss correlate with semantic complexity? |
| 15 | 5.4: Regular Expressions | Ch 5 (finish): Evaluation, loading pretrained weights | L18-19: Semantics (Part 2-3) — truth conditions, entailment | **Notebook 15**: Regex as formal language recognition. Pretrained model evaluation. Semantic entailment testing — can the model recognize when one sentence entails another? Use historical examples. |
| 16 | 5.5: Data Compression | Ch 5 (review) + start Ch 6 | L20-21: Semantics (Part 4-5) — quantifiers, modality | **Notebook 16**: Compression algorithms — BPE IS compression. Full circle from Unit 1. Now explain it with Kolmogorov complexity intuition. Blog post tying compression → tokenization → morphological economy → information theory. |

**Milestone**: Can pretrain a small model. Understand string algorithms deeply. Semantics foundation.
**Blog post**: "BPE Is Compression: Information Theory Meets Morphological Economy"
**DSA level by now**: Comfortable with mediums, starting hards.

---

### Unit 5: Adaptation and Context (Weeks 17-20)

**Theme**: How do systems adapt to specific tasks and contexts?

| Week | Sedgewick | Raschka | MIT 24.900 | Integration Exercise |
|------|-----------|---------|------------|---------------------|
| 17 | 2.1-2.2: Elementary Sorts, Mergesort | Ch 6: Fine-tuning for classification | L22: Dialects | **Notebook 17**: Sorting for ranking results (relevance scoring). Fine-tuning as "learning a dialect" — same base grammar, different surface patterns. Train a classifier to distinguish OE/ME/EModE/ModE text. |
| 18 | 2.3-2.4: Quicksort, Priority Queues | Ch 6 (finish): Classification results | L23: Historical Linguistics | **Notebook 18**: Priority queues for beam search and top-k sampling. Historical linguistics as the "training data" for language change. Sound change as "gradient descent on pronunciation effort." |
| 19 | 2.5: Sorting Applications | Ch 7 (start): Instruction fine-tuning | L24: Endangered Languages | **Notebook 19**: Sorting applications — nDCG calculation requires sorting! Connect to your retrieval quality benchmarks. Instruction fine-tuning begins. Endangered languages as "undertrained models" — low-resource NLP. |
| 20 | 4.1-4.2 (review): Graphs | Ch 7 (cont): RLHF concepts | L25: Language Acquisition | **Notebook 20**: Graph algorithms review (prep for interviews). RLHF as "learning social conventions." Language acquisition vs. pretraining — both learn from distributional evidence, but children have embodiment. |

**Milestone**: Can fine-tune a model. Sorting algorithms solid. Historical linguistics thread complete.
**Blog post**: "Fine-Tuning Is Dialect Acquisition: What Sociolinguistics Tells Us About Transfer Learning"
**DSA level**: Hards in 35 minutes. System design mocks weekly.

---

### Unit 6: Integration and Mastery (Weeks 21-24)

**Theme**: Put it all together.

| Week | Sedgewick | Raschka | MIT 24.900 | Integration Exercise |
|------|-----------|---------|------------|---------------------|
| 21 | 6.1-6.2: Event-driven sim, B-trees | Ch 7 (finish): Instruction fine-tuning complete | L26: Signed Languages | **Notebook 21**: B-trees for large-scale storage. Complete instruction fine-tuning. Signed languages challenge the "language = text" assumption — modality independence. How does this inform multimodal LLMs? |
| 22 | 6.3: Suffix Arrays | Appendix D: Training loop bells and whistles | Review + integration | **Notebook 22**: Suffix arrays for efficient text indexing. Training optimizations. Write the "capstone" blog post that ties the entire journey together. |
| 23 | 6.4-6.5: Maxflow, Reductions | Appendix E: LoRA | Review + integration | **Notebook 23**: Computational complexity — why exact Bayesian inference is intractable (connects to Thompson Sampling as approximation). LoRA as "efficient adaptation." |
| 24 | 6.6: Intractability | Full review | Full review | **Capstone notebook**: The complete "Linguist Builds an LLM" project. Run the full pipeline on a historical English corpus: tokenize → embed → train → fine-tune → evaluate. Annotate every step with linguistic analysis. |

**Milestone**: All three courses complete. Capstone project done. Portfolio pieces published.

---

## Prior Learning Portfolio Structure (SUNY Empire)

### Course 1: Data Structures & Algorithms (3-4 credits)
**Equivalent**: COS 226 / CS 201

**Evidence**:
- 24 notebooks implementing every Sedgewick algorithm in Python
- Big-O analysis on every implementation
- Blog posts explaining algorithmic concepts through linguistic examples
- Neetcode problem log (150+ solved problems with pattern annotations)
- Filmed walkthroughs demonstrating problem-solving process

**Learning outcomes demonstrated**:
- Implement and analyze fundamental data structures (stacks, queues, trees, graphs, hash tables)
- Apply sorting and searching algorithms to real problems
- Analyze time and space complexity
- Recognize algorithmic patterns and select appropriate data structures
- Implement graph algorithms (BFS, DFS, Dijkstra, topological sort)
- Understand string algorithms (tries, regex, compression, substring search)
- Reason about computational intractability and reductions

### Course 2: Introduction to Linguistics (3-4 credits)
**Equivalent**: MIT 24.900 / LING 101

**Evidence**:
- 24 notebooks with linguistic analysis exercises
- Morphological analysis of historical English texts using BPE
- Phonological rule formalization and comparison with attention patterns
- Syntactic parse trees for OE/ME/EModE/ModE texts
- Semantic entailment testing using trained models
- Blog posts connecting linguistic theory to computational implementation
- Historical English corpus analysis (Beowulf → NYT)

**Learning outcomes demonstrated**:
- Analyze morphological structure (affixation, compounding, typology)
- Transcribe and analyze phonetic/phonological systems
- Apply phonological rules and understand their ordering
- Construct syntactic analyses (constituency, dependency)
- Evaluate semantic relationships (entailment, presupposition)
- Discuss language variation (dialects, historical change, endangered languages)
- Connect theoretical linguistics to computational applications

### Course 3: Introduction to Machine Learning / NLP (3 credits)
**Equivalent**: CS 4xx / LING 4xx: Computational Linguistics

**Evidence**:
- Complete GPT implementation from scratch (Raschka chapters 1-7)
- BPE tokenizer implementation with linguistic analysis
- Attention mechanism implementation with syntactic analysis
- Pretraining and fine-tuning on custom corpora
- Evaluation methodology (precision, recall, nDCG — from qortex work)
- LoRA implementation (Appendix E)
- Blog posts explaining LLM internals through linguistic lens

**Learning outcomes demonstrated**:
- Implement neural network components from scratch (embeddings, attention, layer norms)
- Understand and implement the transformer architecture
- Train and evaluate language models
- Fine-tune pretrained models for classification and instruction-following
- Apply tokenization algorithms (BPE) and understand their properties
- Evaluate model performance with appropriate metrics
- Connect model behavior to linguistic theory

### Course 4: Independent Study / Capstone (3 credits)
**Equivalent**: Senior project / Independent study

**Evidence**:
- "A Linguist Builds an LLM from Scratch" blog series (6+ posts)
- Historical English corpus study: BPE → embedding → training → evaluation across 4 periods
- Interactive visualizations (Beta distribution widget, SVG diagrams — already built)
- Twitch stream archive demonstrating teaching ability
- Integration project connecting algorithms, linguistics, and ML
- qortex as prior work demonstrating applied ML research

**Learning outcomes demonstrated**:
- Independently design and execute a research project
- Synthesize knowledge across multiple disciplines
- Communicate technical concepts to varied audiences (blog, video, interactive)
- Apply theoretical knowledge to novel problems
- Produce portfolio-quality artifacts demonstrating mastery

---

## Deliverables Per Unit

Each 4-week unit produces:
1. **4 Jupyter notebooks** — code + analysis + linguistic annotation
2. **1 blog post** — publishable on peleke.dev, connects all three pillars
3. **1 filmed walkthrough** — explains a key concept on camera (performance ramp)
4. **Neetcode problems** — tracked alongside (8-12 per unit per anthropic-ramp schedule)
5. **MIT 24.900 lecture notes** — annotated with computational parallels

## Resources

### Primary Texts
- Sedgewick & Wayne, *Algorithms, 4th Edition* — https://algs4.cs.princeton.edu
- Raschka, *Build a Large Language Model from Scratch* — repo at ../LLMs-from-scratch
- MIT 24.900 OCW — https://ocw.mit.edu/courses/24-900-introduction-to-linguistics-spring-2022/

### Historical English Corpora
- **Old English**: YCOE (York-Toronto-Helsinki), Beowulf (multiple editions)
- **Middle English**: PPCME2, Canterbury Tales (Riverside Chaucer)
- **Early Modern English**: PPCEME, Shakespeare (First Folio), KJB
- **Contemporary English**: Brown Corpus, NYT, Reddit (pushshift)

### Supplementary
- Neetcode.io — DSA practice (see anthropic-ramp.md)
- Sutton & Barto ch. 2-3 — bandits (Thompson Sampling deep-dive)
- Jurafsky & Martin, *Speech and Language Processing* — NLP reference
- CLTK (Classical Language Toolkit) — already used in Interlinear project

### Interview Prep Integration
- Neetcode 150 problems mapped to Sedgewick chapters
- System design mocks using qortex as base system
- ML concept explanations pulled from blog posts
- Performance ramp: camera → recordings → Twitch → mocks
