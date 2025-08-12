# BROKE Cluster 🦫
*Intelligent LLM Cluster for Small Teams on Apple Silicon*

## Status: Alpha Development 🔄

This project is in alpha testing with functional ML-based routing, WebUI, and multi-node cluster support. **Current focus: Architectural research phase** - we're evolving from complexity-only routing toward intelligent resource scheduling for development teams. Major routing philosophy changes are under discussion.

## Vision

Transform your collection of Apple Silicon Macs into an intelligent, self-organizing LLM cluster optimized for small teams (5-10 users) and agent-based workflows.

## The Problem We're Solving

You have:
- A Mac Studio with 192GB RAM sitting idle at night
- A few Mac Minis from various generations  
- Team MacBooks with different memory configurations
- Need for on-premise AI (data privacy, compliance, costs)
- 5-10 people who need LLM access

Current solutions:
- **Cloud APIs**: Expensive, data leaves your premises
- **Single Machine**: Wastes your other hardware
- **Manual Management**: Constant "which model on which machine?" decisions

## Our Solution: Intelligent Routing

BROKE Cluster automatically routes each prompt to the optimal model and node:

```
User: "Analyze this code for security issues"
→ Routes to: Mac Studio M2 Max (64GB) running mixtral:8x7b-instruct

User: "Translate this paragraph to French"  
→ Routes to: Mac Mini M1 (16GB) running phi-3:mini

User: "Generate test data for our API"
→ Routes to: Mac Studio_1 M1 Max (32GB) running mistral:7b-instruct

All transparent with quality indicators (🟢🟡🟠🔴)!
```

## Architecture

```
Your Heterogeneous Mac Network:
┌───────────────┐    ┌──────────────┐    ┌──────────────┐
│ Mac Studio    │    │ Mac Mini M1  │    │ Mac Studio_1 │
│ M2 Max 64GB   │    │ 16GB RAM     │    │ M1 Max 32GB  │
│               │    │              │    │              │
│ Premium Models│    │ Basic Models │    │ Balanced     │
└──────┬────────┘    └─────┬────────┘    └──────┬───────┘
       │                   │                    │
       └───────────────────┴────────────────────┘
                           │
                   ┌───────▼────────┐
                   │  BROKE Router  │
                   │ ML Complexity  │
                   │ Quality Temp   │
                   │ Resource-Aware │
                   └───────┬────────┘
                           │
                 ┌─────────▼─────────┐
                 │ OpenAI-compatible │
                 │  API Endpoints    │
                 └───────────────────┘
```

## Primary Use Case: Multi-Agent Development Teams

BROKE is designed to be the intelligent backend for multi-agent development workflows, providing automatic model routing so agent frameworks can focus on coordination and workflow logic.

**The Vision:**
- Your agent framework makes standard OpenAI API calls
- BROKE intelligently routes each request to the optimal model and node
- Complex architectural discussions → Premium models (Mixtral)
- Code reviews and implementation → Balanced models (Mistral)  
- Quick questions and simple tasks → Basic models (Phi-3)
- Quality indicators (🟢🟡🟠🔴) guide agent confidence levels

**Universal Compatibility:**
Works with any framework that supports OpenAI-compatible APIs: CrewAI, AutoGen, Langchain, custom Python scripts, or direct API calls.

## Target Scenarios

**Perfect for:**
- Small development teams (code analysis stays local)
- Research groups (sensitive data never leaves)
- Creative studios (parallel content generation)  
- Small businesses (avoid monthly cloud costs)

**Not designed for:**
- Large deployments (>20 concurrent users)
- Public-facing services
- 24/7 critical infrastructure

## Technical Approach

- **Model Deployment**: Manually curated MLX models on each node (structured deployment system TODO)
- **Development Tool**: [MLX Knife](https://github.com/mzau/mlx-knife) for model testing and consistency checks during development
- **Routing Logic**: ML-based complexity estimation with resource-aware node selection
- **Communication**: Lightweight message passing (not heavy orchestration)
- **Fault Tolerance**: Graceful degradation when nodes go offline
- **MLX Native**: Optimized for Apple's unified memory architecture

## Components Roadmap

| Component | Status | Description |
|-----------|--------|-------------|
| [MLX Knife](https://github.com/mzau/mlx-knife) | ✅ Released | Single-node model development tool |
| BROKE Router | ✅ Functional | ML complexity routing, quality indicators |
| BROKE WebUI | ✅ Functional | Debug panel, user feedback, routing controls |
| BROKE Monitor | ✅ Integrated | Live routing transparency in WebUI |
| BROKE Agent SDK | 📋 Planned | Python SDK for agent frameworks |
| BROKE Config | ✅ Functional | Multi-node cluster support |

## Development Timeline

- **2025 Q3**: ✅ MLX Knife released
- **2025 Q4**: Alpha with ML routing (technically complete, v1.6.0 in architectural research phase)
- **2026 Q1**: Beta - algorithms mature, tested on available hardware
- **2026 Q2**: Feature-complete beta awaiting hardware evolution
- **2026 H2**: Production readiness depends on Apple Silicon advances + LLM breakthroughs

**Reality Check**: Software will be ready before we know what hardware can truly deliver. We're building the infrastructure for whatever Apple Silicon + MLX can achieve.

## Example Hardware Configurations

**Current Alpha Setup (3-5 users):**
- 1x Mac Studio M2 Max (64GB) - Premium models
- 1x Mac Studio_1 M1 Max (32GB) - Balanced models
- 1x Mac Mini M1 (16GB) - Basic models
- Cost: ~$6,000 (vs $1,000/month cloud)

**Sweet Spot for Small Teams (5-10 users):**
- 1x Mac Studio M4 Max (128GB) - Premium models
- 2x Mac Mini M4 Pro (64GB) - Balanced models  
- 1x Mac Mini M4 (32GB) - Basic models
- Cost: ~$10,000 (vs $2,000/month cloud)

## Technical Challenges

Currently working through:
- Optimal routing algorithms for heterogeneous hardware
- Memory pressure prediction and management
- Agent conversation state distribution
- Grace period handling for node disconnections
- Quality of Service guarantees for concurrent users

## Why BROKE Cluster?

1. **Hardware-Agnostic**: Ready for whatever Apple Silicon delivers
2. **Use What You Have**: That Mac Studio shouldn't be idle
3. **Data Sovereignty**: Your code never leaves your network
4. **Cost Effective**: One-time hardware cost vs eternal cloud fees
5. **Right-Sized**: Built for small teams (5-10 developers), not enterprises
6. **Apple Native**: Optimized for unified memory architecture and MLX

## Get Involved

We're looking for:
- 🧪 **Alpha Testers**: Teams with 3+ Macs willing to test
- 💡 **Use Cases**: Share your team's needs
- 🛠️ **Contributors**: Especially routing algorithm expertise
- 📊 **Benchmarks**: Performance data from your Mac setup

## Community

- 💬 [Discussions](https://github.com/mzau/broke-cluster/discussions) - Ideas & feedback
- 🐛 [Issues](https://github.com/mzau/broke-cluster/issues) - Bug reports (once in alpha)
- ⭐ Star to follow development
- 🔪 Try [MLX Knife](https://github.com/mzau/mlx-knife) today!

## FAQ

**Q: How is this different from Ollama?**  
A: Ollama is great for single machines. BROKE focuses on multi-node routing and agent orchestration. Currently optimized for MLX/Apple Silicon, but the architecture is designed to adapt to evolving hardware platforms.

**Q: Will it support NVIDIA GPUs?**  
A: Currently MLX/Apple Silicon focused. Support for other platforms (NVIDIA, AMD with unified memory, etc.) depends on hardware evolution and community interest. We're building infrastructure that can adapt to whatever hardware delivers the best unified memory experience.

**Q: What about Kubernetes/Docker?**  
A: We're building something simpler. Think "Kubernetes ideas without Kubernetes complexity."

**Q: Can I use this for production?**  
A: Not yet. Target for production-ready: 2026 H2.

**Q: Why "BROKE"?**  
A: Because after buying all these Macs, we're broke! 🦫 As for what BROKE stands for... is it "BROKE Runs On Keen Efficiency"? "Building Robust Orchestrated Knowledge Engines"? "Beaver's Recursive Optimization of Keen Excellence"? The community is still debating! 😄

---

## Contact

- Email: broke@gmx.eu
- Discussions: [Share ideas & feedback](https://github.com/mzau/broke-cluster/discussions)

---

*The BROKE team 🦫 - Building dams to keep your data from flowing to the cloud*

**Note**: This is a research/development project. No warranties, no support guarantees, just beavers doing their best.
