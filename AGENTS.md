# AGENTS.md — LangChain vs LangGraph Learning Notes

## Core Mental Model

- **LangChain** = "Here are the tools. Go figure it out." — LLM controls the flow (agent loop / ReAct)
- **LangGraph** = "Here's the blueprint. Follow it." — You control the flow, LLM does the work
- They're **layers, not competitors**. LangGraph is built on top of LangChain components.

## When to Use LangChain

- Linear pipelines (input → process → output)
- Simple RAG (retrieve → generate)
- Text classification / extraction / summarization
- Single agent with straightforward tool use
- Conversation memory is enough state
- You want minimal boilerplate

## When to Use LangGraph

- Cycles / retry loops / iterative refinement
- Conditional branching (different paths based on results)
- Multi-agent systems (supervisor, debate, swarm)
- Human-in-the-loop (pause for approval mid-execution)
- State persistence (survive crashes, resume later)
- Complex typed state accumulated across many steps
- Need visibility into exactly which step you're on
- Long-running workflows (minutes to hours)

## LangGraph's 3 Primitives

1. **State** — typed dict that flows through the graph (not just messages, arbitrary data with reducers)
2. **Nodes** — functions that do work (read state, return state updates)
3. **Edges** — connections; unconditional or conditional (routing functions that examine state)

## Common Pitfalls

1. Don't use LangGraph for everything — simple chains don't need graph complexity
2. If your prompt has 5 paragraphs of conditional instructions, you've outgrown the agent loop — move control flow to code
3. LangGraph isn't just for multi-agent — single agents with complex workflows benefit too
4. Start simple, graduate to LangGraph when you feel the pain

## Decision Checklist

1. Need loop/retry? → LangGraph
2. Need conditional branching? → LangGraph
3. Need human approval mid-execution? → LangGraph
4. Need state persistence across sessions? → LangGraph
5. Multi-agent system? → LangGraph
6. Linear pipeline or simple agent loop? → LangChain

## Architecture Patterns

- Simple RAG → LangChain LCEL
- Agentic RAG (re-query if irrelevant) → LangGraph
- Supervisor Agent (delegate to workers) → LangGraph
- Simple Tool Agent (think → act → observe) → LangChain AgentExecutor
- Plan-and-Execute (decompose → execute → replan) → LangGraph
