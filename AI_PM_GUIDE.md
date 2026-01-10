# AI Product Manager's Guide to LLM Engineering

## What This Repository Is

This is a hands-on learning repository that teaches you how to build AI products from scratch. Think of it as a practical bootcamp that takes you from "I want to use AI" to "I can build and ship AI products."

## Why This Matters for AI PMs

As an AI PM, you don't need to be an expert engineer, but you **do** need to understand:
- What's actually possible vs. what's hype
- How long things take and what they cost
- How to have technical conversations with your engineering team
- What tradeoffs exist in different approaches

This repo gives you that foundation through building real projects, not just reading about them.

## Core Concepts (First Principles)

### 1. Large Language Models (LLMs) Are Just Pattern Matchers
- They've read billions of documents and learned statistical patterns
- They don't "understand" - they predict what words should come next
- This means: they're great at generating text, terrible at math, and they hallucinate (make up facts)

### 2. There Are Two Ways to Use LLMs

**Option A: Use Someone Else's Model (API)**
- Call OpenAI, Anthropic, Google via their APIs
- Pros: Easy, high quality, no infrastructure needed
- Cons: Costs per request, data privacy concerns, no customization
- **When to use**: MVPs, prototypes, most business applications

**Option B: Run Your Own Model**
- Download and run models like Llama locally or fine-tune them
- Pros: Privacy, no per-request costs, full control
- Cons: Expensive infrastructure, harder to set up, ongoing maintenance
- **When to use**: High-volume products, sensitive data, specific domains

### 3. The AI Product Development Stack

```
User Interface (Gradio, web app)
        ↓
Application Logic (Python)
        ↓
LLM Layer (OpenAI API, Ollama, fine-tuned models)
        ↓
Data Layer (vector databases, knowledge bases)
```

## How to Use This Repo as an AI PM

### Week 1-2: Understanding the Basics (Start Here)
**What you'll learn**: How LLM APIs work, what prompting is, how to build simple AI features

**Key concepts**:
- **Prompts** = Instructions you give to the AI
- **System prompts** = Set the AI's role and behavior
- **User prompts** = The actual question or task
- **Temperature** = Randomness (0 = deterministic, 1 = creative)

**PM takeaway**: You can build a surprising amount with just good prompts and API calls. Most AI features don't need complex ML.

**Try this**:
```bash
cd week1
# Open day1.ipynb in Jupyter or VS Code
# Run through the web summarizer example
```

**What to notice**:
- How simple the code is (this is good!)
- How much the prompt affects the output
- How fast/slow API calls are (important for UX)
- How much it costs per call (check your OpenAI dashboard)

### Week 3-4: Making AI More Reliable
**What you'll learn**: Embeddings, structured outputs, validation

**Key concepts**:
- **Embeddings** = Converting text to numbers so computers can compare meaning
- **Semantic search** = Finding similar concepts, not just matching keywords
- **Structured outputs** = Getting JSON/data instead of free text (critical for products)

**PM takeaway**: The difference between a demo and a product is reliability. These techniques make AI predictable enough to ship.

**Business application**: Search features, recommendation systems, content classification

### Week 5: RAG - Making AI Know Your Company's Information
**What you'll learn**: Retrieval Augmented Generation (RAG)

**What is RAG in plain English**:
1. Store your company's documents in a database
2. When user asks a question, find relevant documents
3. Give those documents to the LLM as context
4. LLM answers using your actual data

**Why this matters**:
- LLMs don't know about your products, customers, or internal processes
- RAG lets you give them this knowledge without retraining (expensive!)
- This is how 80% of enterprise AI products work

**PM takeaway**: RAG is your go-to solution for "AI that knows our business." It's fast to build, works well, and relatively cheap.

**Cost comparison**:
- Fine-tuning a model: $500-$10,000+, weeks of work
- RAG system: $10-$100/month, days to build

**Try this**:
```bash
cd week5
# Run day1.ipynb - simple RAG system
# Run day5.ipynb - production-quality RAG
```

**What to notice**:
- How chunking affects answer quality (bigger chunks = more context but less precise)
- How many documents it retrieves (tradeoff: cost vs. accuracy)
- What happens when the answer isn't in your documents

### Week 6: Evaluation - Know If Your AI Actually Works
**What you'll learn**: Data curation, testing, evaluation frameworks

**The hard truth**: You can't just "feel" if your AI is good. You need metrics.

**Key concepts**:
- **Test sets** = Representative examples of real user questions
- **Baselines** = Simple solutions to compare against (often ML isn't needed!)
- **Evaluation metrics** = Quantifiable measures of success

**PM framework for AI evaluation**:
1. **Define success metrics** (accuracy, response time, cost per query)
2. **Create a test set** (100+ real examples with known good answers)
3. **Measure current performance** (baseline)
4. **Test changes** (new prompts, models, approaches)
5. **Compare** (did it actually get better?)

**PM takeaway**: "It feels better" isn't good enough. Build evaluation into your workflow from day 1.

### Week 7: Fine-Tuning - When RAG Isn't Enough
**What you'll learn**: How to train an LLM on your specific task

**When to fine-tune**:
- ✅ You have 1,000+ high-quality examples
- ✅ Consistent task (e.g., always classify support tickets)
- ✅ RAG/prompting isn't good enough
- ✅ You need specific tone/style/format

**When NOT to fine-tune**:
- ❌ You just need the AI to know your data (use RAG)
- ❌ You have <500 examples (not enough data)
- ❌ Task keeps changing (fine-tuning is expensive to redo)
- ❌ Good prompting works (simplest solution wins)

**PM takeaway**: Fine-tuning is powerful but expensive and slow. Exhaust simpler options first.

### Week 8: Multi-Agent Systems - The Future
**What you'll learn**: Building systems where multiple AI agents work together

**What are agents**:
- Traditional: LLM → Response
- Agents: LLM → Think → Use tools → Think → Use tools → Response

**The "Price is Right" project shows**:
- Planning agent (decides what to do)
- Scanner agent (processes input)
- Frontier agent (uses GPT-4 for hard problems)
- Neural network agent (uses ML for predictions)
- Ensemble agent (combines multiple answers)
- Evaluator agent (checks quality)

**PM takeaway**: Agents are cutting-edge but complex. Great for autonomous systems, probably overkill for most products in 2025.

## Practical PM Workflows

### Building an MVP (Week 1-2 Techniques)
1. Define the problem clearly
2. Write example inputs and ideal outputs
3. Test with GPT-4 via API (just paste in ChatGPT first!)
4. Write a system prompt that works
5. Build minimal UI with Gradio
6. Test with 5-10 real users
7. Iterate on prompt based on failures

**Timeline**: 1-3 days
**Cost**: <$5

### Building a Production Feature (Week 5 Techniques)
1. Collect your company's relevant documents
2. Build RAG system (use week5/pro_implementation as template)
3. Create test set of 100 real questions
4. Measure accuracy/quality
5. Tune retrieval (chunk size, number of results)
6. Add error handling and fallbacks
7. Deploy with monitoring

**Timeline**: 1-2 weeks
**Cost**: $50-500/month depending on usage

### Improving an Existing AI Feature
1. **Measure current performance** (week 6 evaluation techniques)
2. **Analyze failures** (where does it fail? why?)
3. **Hypothesis** (will better prompts/RAG/fine-tuning help?)
4. **Implement cheapest solution first**:
   - Better prompts → free, instant
   - RAG → cheap, fast (days)
   - Fine-tuning → expensive, slow (weeks)
5. **Measure new performance**
6. **Ship if better, iterate if not**

## Cost Management (Critical for PMs)

### API Costs (OpenAI Example)
- GPT-4.1-nano: ~$0.01 per 1,000 requests (simple tasks)
- GPT-4.1-mini: ~$0.05 per 1,000 requests (complex tasks)
- GPT-4o: ~$0.30 per 1,000 requests (when you need the best)

**PM math**:
- 10,000 users/month × 10 queries each = 100,000 queries
- With gpt-4.1-mini: ~$5/month
- With gpt-4o: ~$30/month

**Cost optimization strategies**:
1. Use cheaper models for simple tasks (classification, extraction)
2. Use expensive models only for complex reasoning
3. Cache common responses
4. Set rate limits per user
5. Consider Ollama (free, local) for high-volume simple tasks

### Infrastructure Costs
- **RAG system**: $20-200/month (vector database + hosting)
- **Fine-tuned model**: $500-5,000 upfront + $50-500/month hosting
- **Agent system**: $100-1,000/month (multiple models + orchestration)

## Common PM Questions

### "How do I know if we need AI for this feature?"
Ask:
1. Is there a simpler rule-based solution? (if yes, do that first)
2. Do we have examples of inputs and desired outputs? (need this for AI)
3. Is 80% accuracy good enough? (AI won't be perfect)
4. Can we afford hallucinations/errors? (if no, add human review)

### "Which model should we use?"
Decision tree:
1. **Prototype/MVP**: GPT-4.1-mini (fast, cheap, good)
2. **Production, simple tasks**: GPT-4.1-nano or Llama 3.2
3. **Production, complex reasoning**: GPT-4o or Claude Sonnet
4. **High volume, simple tasks**: Fine-tuned Llama or Ollama
5. **Sensitive data**: Ollama (runs locally, never leaves your servers)

### "How long will this take to build?"
Rough estimates:
- Simple chatbot: 1-3 days
- RAG system: 1-2 weeks
- Fine-tuned model: 3-6 weeks
- Multi-agent system: 2-3 months

### "What could go wrong?"
Top risks:
1. **Hallucinations**: AI makes up facts confidently
   - Mitigation: RAG, citations, human review
2. **Cost explosion**: Usage 10x higher than expected
   - Mitigation: Rate limiting, caching, monitoring
3. **Latency**: Responses too slow for good UX
   - Mitigation: Streaming, smaller models, caching
4. **Prompt injection**: Users trick AI into ignoring instructions
   - Mitigation: Input validation, separate system/user prompts
5. **Data leakage**: AI reveals training data or other users' data
   - Mitigation: Use APIs correctly, test thoroughly

## Your Learning Path

### Phase 1: Understanding (Weeks 1-2)
**Goal**: Speak the language, understand what's possible

**Action items**:
- [ ] Run week1/day1.ipynb completely
- [ ] Modify the prompts and see what changes
- [ ] Check your OpenAI usage dashboard
- [ ] Try 3 different models (GPT-4.1-nano, GPT-4.1-mini, GPT-4o)
- [ ] Build a simple prototype for your product idea

**You'll know you're ready when**: You can explain to your team how an LLM API call works and roughly what it costs.

### Phase 2: Building (Weeks 3-5)
**Goal**: Build a production-quality feature

**Action items**:
- [ ] Understand embeddings (week3)
- [ ] Build a RAG system with your company's data (week5)
- [ ] Create a test set of 50+ real questions
- [ ] Measure and improve accuracy
- [ ] Add a simple Gradio UI

**You'll know you're ready when**: You've shipped an AI feature to real users and can explain why it works (or doesn't).

### Phase 3: Mastery (Weeks 6-8)
**Goal**: Make informed decisions on complex AI architecture

**Action items**:
- [ ] Build evaluation framework (week6)
- [ ] Understand when fine-tuning makes sense (week7)
- [ ] Explore agent architectures (week8)
- [ ] Compare costs/benefits of different approaches

**You'll know you're ready when**: Engineers ask your opinion on technical AI decisions and you can contribute meaningfully.

## Key Files to Reference

### For Product Decisions
- `guides/09_ai_apis_and_ollama.ipynb` - When to use which model
- `week5/day1.ipynb` - RAG basics
- `week6/day1.ipynb` - Evaluation frameworks

### For Technical Discussions
- `week1/day1.ipynb` - Basic API structure
- `week5/pro_implementation/` - Production RAG example
- `week8/agents/` - Agent architecture patterns

### For Cost/Timeline Estimates
- `README.md` - API cost monitoring links
- `pyproject.toml` - Full technology stack

## Final Advice

1. **Start simple**: Most AI products are just good prompts + API calls. Don't overcomplicate.

2. **Measure everything**: You can't improve what you don't measure. Build evaluation early.

3. **User test constantly**: AI surprises you. What works in demos fails with real users and vice versa.

4. **Plan for failure**: AI will fail. Design UX for graceful degradation (fallbacks, human review, clear confidence signals).

5. **Avoid hype**: Not every feature needs AI. Not every AI feature needs agents. Not every agent needs fine-tuning. Simple > complex.

6. **Focus on the problem**: The best AI products solve real problems extremely well. The technology is just a means to that end.

## Getting Started Right Now

```bash
# 1. Set up environment
cd "Desktop/Tech Skills/llm_engineering"
python -m venv .venv
source .venv/bin/activate  # On Mac/Linux
# .venv\Scripts\activate   # On Windows

# 2. Install dependencies
pip install -r requirements.txt

# 3. Add your OpenAI API key to .env file
echo "OPENAI_API_KEY=sk-proj-your-key-here" > .env

# 4. Start with Week 1
cd week1
# Open day1.ipynb in VS Code or Jupyter
```

## Questions to Ask Yourself

After each week, reflect:
- What surprised me?
- What's easier than I thought?
- What's harder than I thought?
- How could I apply this to [your product]?
- What would I need to convince my team to try this?

## Remember

You don't need to be able to code this from scratch. You need to:
- Understand what's possible
- Ask the right questions
- Make informed tradeoffs
- Guide your team effectively

This repo gives you that foundation. Now go build something.
