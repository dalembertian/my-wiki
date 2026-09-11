# AI Agent Learning Roadmap and Resources

A consolidated reference from five short source pieces — mostly bullet-list roadmaps and reading lists, thin individually but useful together as one lookup page. (Note: "Github Vibe Coding Roadmap" and "Vibe coding: Your roadmap to becoming an AI developer" turned out to be the same GitHub blog post saved as two separate source files — merged here rather than duplicated.)

## A minimal personal roadmap

A compact 7-step path: Python → LLM fundamentals (tokens, prompts, embeddings, context windows) → LLM APIs (GPT, Claude, Gemini, Ollama) → chaining tools/responses (LangGraph, PydanticAI, for multi-step agents) → memory/context (vector databases like Pinecone) → tools and function calling (connecting to external APIs) → deployment (wrapping the agent in a web API framework like FastAPI).

## GitHub's broader "become an AI developer" roadmap

- **Core languages**: Python (the field's default — overtook JavaScript as GitHub's #1 language in 2024), Java (enterprise-scale systems), C++ (performance-critical work like robotics/real-time simulation).
- **Core frameworks**: TensorFlow, Keras (built on TensorFlow, fast prototyping), PyTorch (favored by researchers, dynamic computation graphs), Scikit-learn (classical ML).
- **Build a public portfolio**: organized repos with clear READMEs, a profile README, GitHub Pages for demos/case studies, visible open-source contributions.
- **GitHub Copilot certification** as a credential signaling AI-tooling fluency to employers.

## Workshop topic map (4-hour Krohn/Donner agentic AI video)

Defines agents as systems where **LLM output controls the workflow** (vs. fixed, predefined pipelines); covers core building blocks (tools, and the risks — unpredictability, cost — that come with them, plus monitoring/guardrails); frameworks compared: **MCP** ("USB-C for agentic applications" — a connectivity standard, not a framework itself), **OpenAI Agents SDK** (lightweight, flexible), **CrewAI** (heavier-weight, multi-agent-focused, "batteries included" safe code execution), **LangGraph** and **Microsoft AutoGen** (more complex orchestration). Five workflow design patterns taught: **prompt chaining, routing, parallelization, orchestrator-worker, evaluator-optimizer**. Hands-on exercises: recreating OpenAI's Deep Research with the Agents SDK, building an autonomous software-engineering team with CrewAI, and building simulated autonomous traders using MCP for real-time data access.

## Curated reading list (by topic)

- **Math foundations**: *Mathematics of Machine Learning* (Danka)
- **Programming & stats**: *Fluent Python* (Ramalho), *ML with PyTorch and Scikit-Learn* (Raschka), *Practical Statistics for Data Scientists* (Bruce & Bruce)
- **NLP**: *Python NLP Cookbook* (Antić & Chakravarty), *NLP with Transformers* (Tunstall, von Werra, Wolf)
- **ML systems**: *Designing Machine Learning Systems* (Huyen), *ML System Design Interview* (Aminian & Xu)
- **LLMs & agents**: *AI Agents in Practice* (Alto), *AI Engineering* (Huyen)
- **Infrastructure**: *Designing Data-Intensive Applications* (Kleppmann)

## Cross-reference

For a durable *concept* rather than a resource list, see [[Agent Memory - Semantic, Episodic, and Procedural]] (split out separately since the memory taxonomy is worth citing on its own). For hands-on architecture, see [[A Minimal Coding Agent Harness in Python]], [[Function Calling and Tool Use in LLMs]], and [[Harness Engineering - Guides, Sensors, and Regulation Categories]].


[[The Harness Is the Product]] points here as the learning path for building the layer it describes.

## Sources
- [Learning AI Agents](<../../source/Learning AI Agents.md>)
- [Agentic AI Hands-On in Python A Video Tutorial](<../../source/Agentic AI Hands-On in Python A Video Tutorial.md>)
- [Books About AI](<../../source/Books About AI.md>)
- [Github Vibe Coding Roadmap](<../../source/Github Vibe Coding Roadmap.md>)
- [Vibe coding Your roadmap to becoming an AI developer](<../../source/Vibe coding Your roadmap to becoming an AI developer.md>)

#ai-techniques #agents #learning-resources
