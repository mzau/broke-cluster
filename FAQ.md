# BROKE Cluster - Frequently Asked Questions

*Comprehensive Q&A for understanding BROKE's positioning and technical decisions*

## Architecture & Positioning

### Q: How does BROKE compare to LLM routing frameworks like RouteLLM/OptiLLM?

**A: BROKE and RouteLLM solve different problems with complementary approaches:**

**RouteLLM/OptiLLM** (Cloud-to-Cloud Cost Optimization):
- **Goal**: Route between different cloud APIs (GPT-3.5 vs GPT-4)
- **Strategy**: "Cheap cloud vs. expensive cloud"
- **Savings**: Up to 85% cost reduction
- **Trade-off**: Quality degradation for cost savings
- **Context**: All queries go to external cloud providers

**BROKE Cluster** (Local-First with Strategic Cloud Integration):
- **Goal**: Minimize cloud dependency through intelligent local orchestration
- **Strategy**: "Local-first with selective expert escalation"
- **Savings**: Potentially significant cost reduction through local processing
- **Trade-off**: Hardware investment vs. operational costs
- **Context**: Most queries processed locally, selective cloud escalation

**Theoretical Cost Comparison:**
```
Example scenario - 1000 queries/month:

RouteLLM:
- 850 queries → cheap cloud ($0.85)
- 150 queries → expensive cloud ($4.50)
Total: $5.35/month (85% savings vs. all-premium)

BROKE (theoretical):
- ~900 queries → local (hardware amortization cost)
- ~100 queries → strategic cloud ($3.00)
Hardware: $4000 Mac Studio amortized over time
Per-user costs vary significantly based on team size and utilization

*Note: Actual savings depend on team size, usage patterns, 
and hardware utilization. Empirical validation needed.*
```

**When to choose what:**
- **RouteLLM**: Cloud-first teams, individual usage, no hardware investment
- **BROKE**: Teams with privacy needs, hardware available, local expertise development

### Q: What happens when the team can't converge on a solution locally?

**A: That's outside BROKE's core responsibility - it's handled at the use-case level:**

**BROKE's Scope**: Intelligent routing of individual queries to optimal local models with quality guidance. BROKE doesn't manage team workflows or decide when to escalate to external resources.

**Where Escalation Happens**: In your use-case implementation (e.g., CrewAI setup, custom workflows):
- **CrewAI Agents**: Can be configured with different LLM endpoints - some pointing to BROKE, others to cloud APIs
- **Custom Workflows**: Application logic decides when local resources are insufficient
- **Manual Decision**: Team members can directly use cloud services when needed

**BROKE's Contribution**: Provides quality indicators (🟢🟡🟠🔴) that help teams and frameworks make informed decisions about when local solutions might be insufficient.

**Example in Practice:**
```python
# In CrewAI - you configure escalation, not BROKE
lead_architect = Agent(
    role="Critical Decisions", 
    llm=OpenAI(base_url="https://api.openai.com")  # Direct cloud
)

code_reviewer = Agent(
    role="Routine Tasks",
    llm=OpenAI(base_url="http://localhost:8000")    # BROKE local routing
)
```

**Philosophy**: BROKE focuses on being the best possible local router. Escalation strategies are use-case specific and should remain flexible.

### Q: How does BROKE handle multi-agent frameworks like CrewAI?

**A: BROKE is designed as the intelligent backend for agent orchestration:**

**Universal Compatibility:**
- Any framework using OpenAI-compatible APIs works seamlessly
- CrewAI, AutoGen, Langchain, custom scripts - no framework changes needed
- Agents make standard API calls, BROKE handles intelligent routing

**Agent-Optimized Features:**
- **Quality Temperature**: Agents get confidence indicators (🟢🟡🟠🔴) for decision-making
- **Context Continuity**: Full conversation history maintained across model switches
- **Team Intelligence**: Learns patterns from multi-agent interactions
- **Resource Scheduling**: Parallel agent requests handled efficiently

**Example Agent Assignment Flexibility:**
```python
# In CrewAI - you CAN assign cloud APIs to specific agents
lead_architect = Agent(
    role="Lead Architect", 
    llm=OpenAI(base_url="https://api.openai.com")  # Direct cloud for critical decisions
)

code_reviewer = Agent(
    role="Code Reviewer",
    llm=OpenAI(base_url="http://localhost:8000")    # BROKE for routine reviews
)
```

**BROKE's Value**: Intelligent routing reduces need for manual agent-to-model assignments while providing transparency for agent decision-making.

## Technical Architecture

### Q: How does BROKE's multi-dimensional complexity work?

**A: Evolution from 1D to 3D complexity understanding:**

**Previous Approach** (Linear 0.0-1.0 scale):
```python
complexity = estimate_complexity(prompt)  # Single number
if complexity < 0.5:
    route_to_basic_model()
```

**Current Approach** (Multi-dimensional ML):
```python
complexity_estimate = {
    "prompt_complexity": 0.75,    # Question sophistication
    "answer_complexity": 0.80,    # Expected response depth  
    "language_bias": 0.1,         # Code vs. prose weighting
    
    # Future dimensions:
    "domain_rust": 0.95,          # Domain expertise required
    "reasoning_chains": 0.60,     # Logical reasoning depth
    "creative_design": 0.10       # Creative vs. analytical
}
```

**Quality Temperature System:**
- Green (0.0-0.2): High confidence, use result directly
- Yellow (0.3-0.5): Good quality expected, minor review
- Orange (0.6-0.7): Caution zone, review critical details
- Red (0.8-1.0): Maximum caution, treat as draft

**Benefits:**
- **Transparent Quality Guidance**: Users know what to expect
- **No Binary Rejections**: Always get an answer with appropriate warnings
- **Continuous Learning**: User feedback improves future routing

### Q: What is N:1 Model Tier Support?

**A: Flexible model assignment within quality tiers:**

**Traditional 1:1 Mapping:**
```json
{
  "basic_tier": ["phi-3-mini"],
  "balanced_tier": ["mistral-7b"], 
  "premium_tier": ["mixtral-8x7b"]
}
```

**BROKE's N:1 Flexible Tiers:**
```json
{
  "basic_tier": ["phi-3-mini"],
  "balanced_tier": ["mistral-7b", "llama-8b"], 
  "premium_tier": ["mixtral-8x7b", "llama-3.3-70b"]
}
```

**Intelligent Selection Within Tier:**
- Resource availability (which model is loaded?)
- Queue lengths (which model is free?)
- Domain expertise (which model handles Rust better?)
- User preferences (learned patterns)

**Configuration-Driven:**
```bash
# Current: 1:1 mapping
export BROKE_CONFIG_DIR=config/3nodes

# Future: Flexible N:1 mapping  
export BROKE_CONFIG_DIR=config/3n4m  # 3 nodes, 4 models
```

### Q: How does Universal Data Collection work?

**A: MultiPL-E compatible training data from all interactions:**

**Data Format** (compatible with standard benchmarks):
```json
{
  "name": "broke_production_2025_001",
  "language": "rust",
  "prompt": "Implement ownership-safe linked list...",
  "tests": "assert_eq!(list.len(), 1);",
  
  "broke_metadata": {
    "source": "production_webui",
    "model_used": "mistral-7b",
    "quality_temperature": 0.3,
    "user_rating": 4,
    "response_time_seconds": 4.2,
    "routing_method": "ml_complexity_estimator",
    "dimensions": {
      "prompt_complexity": 0.75,
      "domain_rust": 0.95
    }
  }
}
```

**Collection Sources:**
- WebUI user interactions (with ratings)
- Benchmark executions (`make benchmark`)
- Agent framework usage
- Manual curation

**Benefits:**
- **Continuous Learning**: Model performance improves over time
- **Benchmark Integration**: Data can be used in standard academic benchmarks  
- **Privacy-Preserving**: Only technical patterns collected, no business logic
- **Research Contribution**: Anonymous data can contribute to LLM routing research

## Use Cases

### Q: Why focus on software development instead of general multimodal AI?

**A: Software development is the most security-sensitive use case requiring on-premises control.**

- Source code and IP must stay local (no cloud leakage)
- Development workflows need predictable, controlled environments  
- Agent-based patterns emerge naturally for code review, testing, documentation

**Why not multimodal:** Images/video less security-critical; general chat can safely use cloud services.

### Q: What specific development workflows benefit from BROKE's routing?

**A: Code-intensive tasks where different models excel at different complexity levels:**

- **Fast models:** Code formatting, simple documentation, unit test generation
- **Balanced models:** Code review, API integration, debugging assistance  
- **Premium models:** Architecture design, complex refactoring, performance optimization

**Agent collaboration:** Multiple AI assistants working on different aspects of the same project.

### Q: Is BROKE suitable for multi-agent development workflows?

**A: Planned for Q4/2025 with appropriate safety considerations.**

Agent-driven development requires careful isolation to prevent unintended system modifications. Current focus: single-user team routing. Multi-agent orchestration is a future milestone.

## Implementation & Usage

### Q: What's the current status and roadmap?

**A: Alpha functional, architectural research phase:**

**Current Status (v1.5.8-beta):**
- ✅ ML-based complexity routing working
- ✅ WebUI with quality indicators  
- ✅ Multi-node cluster support (3 nodes tested)
- ✅ Universal data collection
- ⚠️ Architectural evolution in progress (complexity-only → resource-aware)

**Next Steps (v1.5.9):**
- Team convergence detection
- Resource-aware routing improvements  
- Quality temperature WebUI integration
- Multi-agent benchmark suite (NetHack development team simulation)

**Vision (v1.6.0+):**
- Hybrid local-cloud architecture
- Team pattern learning
- Predictive model loading
- True team productivity optimization

### Q: Will BROKE support multimodal AI (images, documents)?

**A: Via plugin architecture - letting the community drive which modes matter most.**

**Why plugins:** Rather than guessing which multimodal features developers need, we'll provide a plugin system. If OCR for document analysis proves critical, someone will build it. If image generation seems appealing, the community can contribute it.

**Philosophy:** Market-driven feature development over engineering assumptions. The community decides what's "sexy" enough to implement.

**Core focus:** Text-based routing remains our specialty and development priority.

### Q: What hardware do I need to get started?

**A: Minimum viable setup for testing:**

**Minimal Setup** (1-3 users):
- 1x Mac with 32GB+ RAM (M1 Pro/Max or better)
- MLX-compatible models (4-bit quantized)
- Local network (no internet required for basic operation)

**Recommended Team Setup** (5-10 users):
- 1x Mac Studio M2/M4 Max (64GB+) - Premium models
- 1-2x Mac Mini M4 Pro (32GB) - Balanced models
- 1x Mac Mini M4 (16GB) - Basic models
- Gigabit local network for model coordination

**Future-Proof Investment:**
- Mac Studio M4 Max (128GB) when available
- Multiple Mac Mini M4 Pro for horizontal scaling
- ROI depends on team size, usage patterns, and cloud alternatives

### Q: I want to try this TODAY. What can I do?

**A: Start with MLX Knife - the production-ready foundation component:**

**Quick Start:**
1. **Clone MLX Knife**: 
   ```bash
   git clone https://github.com/mzau/mlx-knife
   cd mlx-knife
   ```
2. **Install dependencies**: `pip install -r requirements.txt`
3. **List available models**: `python -m mlx_knife list`
4. **Start API server**: `python -m mlx_knife server --port 8000`
5. **Open simple chat interface**: `open simple_chat.html` (or navigate in browser)
6. **Alternative**: Test with any OpenAI-compatible client via curl/API calls

**What you get:**
- Single-node model management and serving
- OpenAI-compatible API endpoint  
- Model performance testing and validation
- Foundation for understanding what BROKE Cluster will orchestrate

**Next Steps:**
- Try different models: `mlxk run mistral-7b-instruct "your prompt"`
- Monitor performance: `mlxk health mistral-7b-instruct`
- Join discussions to share your single-node experience

**Reality Check:** This is what BROKE Cluster will intelligently route between multiple nodes. MLX Knife gives you the single-node experience today while BROKE's multi-node orchestration is in alpha development.

### Q: How do I contribute or get involved?

**A: Multiple ways to engage:**

**For Users:**
- Star the repository to follow development
- Join [Discussions](https://github.com/mzau/broke-cluster/discussions) for feedback
- Try [MLX Knife](https://github.com/mzau/mlx-knife) for single-node testing

**For Alpha Testers:**
- Teams with 2+ Mac systems willing to test
- Share your multi-agent use cases and requirements
- Provide performance benchmarks from your hardware

**For Contributors:**
- ML routing algorithm improvements
- Agent framework integration examples
- Hardware configuration optimization
- Documentation and tutorials

**For Researchers:**
- Academic collaboration on LLM routing strategies
- Multi-dimensional complexity research
- Team productivity optimization studies

## Comparisons & Context

### Q: How is this different from Ollama?

**A: Ollama is great for single machines. BROKE focuses on multi-node routing and agent orchestration:**

**Ollama** (Excellent single-node management):
- ✅ Great for individual machines
- ✅ Easy model management and serving  
- ✅ Production-ready for single-node use
- ❌ No intelligent multi-node routing
- ❌ No complexity-aware model selection
- ❌ Manual coordination between nodes

**BROKE** (Multi-node intelligent orchestration):
- ✅ Automatic routing across heterogeneous hardware
- ✅ ML-based complexity estimation and quality guidance
- ✅ Team-aware resource scheduling
- ✅ Agent-optimized serving
- ⚠️ More complex setup (alpha software)

**Platform Strategy:**
Currently optimized for MLX/Apple Silicon, but the architecture is designed to adapt to evolving hardware platforms.

**Complementary Usage:**
- Ollama for individual development and testing
- BROKE for team coordination and production workflows
- MLX Knife for development and model validation

### Q: Will local LLMs be competitive with cloud models by the time BROKE reaches production?

**A: This is the fundamental uncertainty driving our development approach:**

**The Core Question:** 
Will local hardware advances (Apple M-series, unified memory, MLX optimization) keep pace with cloud model improvements by 2026-2027 timeframe?

**Optimistic Scenario** (BROKE team's working hypothesis):
- M4/M5 Max with 192GB+ unified memory
- MLX framework maturity enables efficient inference
- Local 70B+ parameter models match today's cloud capabilities
- **Result**: Local-first architecture becomes highly viable for software development

**Pessimistic Scenario** (realistic possibility):
- Cloud models advance faster than local hardware/efficiency
- Local models remain 6-12 months behind cloud state-of-the-art
- **Result**: BROKE becomes specialized tool for privacy/cost scenarios only

**Pragmatic Approach:**
We're building the **infrastructure** for whatever hardware delivers, rather than betting on specific performance outcomes. Key principles:

- **Hardware-Agnostic Architecture**: Ready to adapt to best unified memory platform
- **Hybrid-Ready**: Can integrate cloud escalation if local capabilities plateau
- **Focus on Team Intelligence**: Even if models are equivalent, team orchestration adds value
- **Privacy-First**: Local processing valuable regardless of performance parity

**Reality Check:** Software will likely be ready before we know what hardware can truly deliver. We're building the routing and orchestration layer for whatever the local/cloud balance becomes.

### Q: Why focus on Apple Silicon / MLX specifically?

**A: Strategic platform choice, but architecture is hardware-agnostic:**

See detailed hardware strategy discussion above. In short: unified memory advantages today, but BROKE adapts to whatever platform delivers best price/performance for teams.

### Q: Will it support NVIDIA GPUs?

**A: Future platform support depends on unified memory evolution:**

**Current Focus:** Apple Silicon + MLX (proven unified memory advantage)
**Future Candidates:** NVIDIA (when unified memory matures), AMD (strong potential)
**Decision Criteria:** Whatever delivers best price/performance for small teams

### Q: What about Kubernetes/Docker?

**A: Apple Silicon GPU performance requires native execution:**

**Technical Reality (2025):**
- MLX/Metal needs native macOS GPU access for performance
- Containers would force CPU-only execution (~10x slower)
- Apple's container platform still in early development

**Architectural Philosophy:** "Kubernetes ideas without Kubernetes complexity" - intelligent routing for small teams (5-10 users), not enterprise container orchestration.

**BROKE Scope:** Native multi-node MLX inference only. Container deployment is out-of-scope for the core server.

### Q: Can I use this for production?

**A: Not yet - alpha software with production timeline 2026 H2:**

**Current Status:** Functional alpha with architectural research ongoing
**Target Timeline:** Production-ready features expected 2026 H2
**Dependencies:** Apple Silicon hardware evolution + LLM performance improvements

**Use Today For:** Research, development, alpha testing with realistic expectations

### Q: Why "BROKE"?

**A: Because after buying all these Macs, we're broke! 🦫**

As for what BROKE stands for... is it "BROKE Runs On Keen Efficiency"? "Building Robust Orchestrated Knowledge Engines"? "Beaver's Recursive Optimization of Keen Excellence"? The community is still debating! 😄

**The beaver mascot** represents building dams to keep your data from flowing to the cloud.

---

## Contact & Community

**Questions not covered here?**
- 💬 [GitHub Discussions](https://github.com/mzau/broke-cluster/discussions)
- 📧 broke@gmx.eu  
- 🐛 [Issues](https://github.com/mzau/broke-cluster/issues) for bug reports

**Stay Updated:**
- ⭐ Star the repository
- 🔔 Watch for releases
- 🦫 Follow the beaver brigade!

---

*Last Updated: 2025-08-23 - Living document, updated as project evolves*