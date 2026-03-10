# Problem Statement: Disease Information Agent for Multi-Client Epi Services

Applied epidemiology teams fielding disease information requests spend significant time repeatedly researching, synthesizing, and reformatting the same core epi content for different clients. Each client has distinct risk contexts, terminology preferences, and output format requirements — making manual adaptation slow and hard to scale across a growing portfolio of disease topics and stakeholders.

**The goal:** Build an LLM-powered disease information agent with built-in, client-aware prompts that automatically generates accurate, consistently structured disease summaries on demand — reducing researcher burden and enabling faster, scalable response across clients.

**Why this is a strong LLM use case:**
- High repetition across similar requests
- Well-defined source material (CDC, WHO, peer-reviewed surveillance data)
- Output structure is predictable and templatable
- Client customization is a prompt/context problem, not a knowledge problem
