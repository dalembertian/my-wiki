# RAG vs. Fine-Tuning

## RAG: retrieve, augment, generate

**Retrieval-augmented generation** connects an LLM to an external knowledge source at query time: retrieve relevant documents/snippets → augment the prompt with them → generate a grounded response. **Traditional RAG** does this in one static step against pre-indexed data (good for well-organized, straightforward lookups). **Agentic RAG** iterates — searching, evaluating, and refining its own retrieval based on what it finds — suited to multi-step problems a single retrieval can't resolve. Benefits: real-time freshness, reduced hallucination (retrieved facts act as a check), and cheap updates (swap the knowledge base, no retraining).

## Fine-tuning: bake the knowledge in

**Fine-tuning** further trains a pre-trained model on curated, domain-specific data. Three flavors: **full fine-tuning** (all parameters, most comprehensive, most expensive), **parameter-efficient fine-tuning** (a subset of parameters, cheaper), and **continuous pretraining** (extends training on new data while retaining prior knowledge). Benefits: real domain expertise (generalist → medical/legal/support specialist) while retaining broad capability, and — unlike RAG — no retrieval infrastructure or per-query latency overhead.

## When to use which

| | RAG | Fine-tuning |
|---|---|---|
| Best for | frequently updated, dynamic data, rapid deployment | stable, specialized domains needing high accuracy |
| Upfront cost | lower (no retraining) | higher (training compute) |
| Ongoing cost | retrieval infrastructure | periodic retraining as domain evolves |
| Example use cases | sales enablement, IT help desks, financial analysis, HR FAQs | healthcare chatbots, technical document summarization, compliance monitoring, sentiment analysis |

## Hybrid: both together

A fine-tuned model can still use RAG for real-time facts — specialized domain understanding plus current information. The tradeoff: hybrid systems need both a robust retrieval pipeline *and* a well-curated fine-tuning dataset, meaningfully raising infrastructure complexity, compute cost, and data-governance burden versus either approach alone.

## Challenges

- **RAG**: data privacy/source integrity (is the retrieved source trustworthy?), retrieval bias (inherits whatever bias the source corpus has), validation difficulty (dynamically retrieved content is harder to verify than static knowledge), and latency from real-time lookups.
- **Fine-tuning**: expensive, time-consuming data curation; overfitting risk (performs well on training data, generalizes poorly); high training compute cost; ongoing retraining burden as the domain evolves; and poor scaling across multiple domains (each needs its own fine-tuning run).

## Cross-reference

Both techniques exist because of the shift described in [[From Data-Driven Software to Generative AI]]: once a general model can be adapted rather than rebuilt, "retrieve or retrain" replaces "design a new architecture" as the central domain-fit question. [[A Document-Grounded Rules Referee via Prompt Engineering]] is the third option neither column covers — bounded domains where a sufficiently disciplined prompt over uploaded documents substitutes for a retrieval pipeline entirely.


[[AI Digital Fossils in LLM Training Data]] is a concrete instance of the source-integrity risk listed above, and of why baking knowledge into weights is hard to undo.

## Sources
- [AI - RAG vs fine-tuning](<../../source/AI - RAG vs fine-tuning.md>)

#ai-fundamentals #rag #fine-tuning #llm
