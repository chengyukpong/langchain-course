# AGENTS.md — LangChain vs LangGraph vs Deep Agents Learning Notes

## Core Mental Model

- **LangChain** = "Here are the tools. Go figure it out." — LLM controls the flow (agent loop / ReAct)
- **LangGraph** = "Here's the blueprint. Follow it." — You control the flow, LLM does the work
- **Deep Agents** = "Here's a capable assistant with its own desk, filing cabinet, and subordinates." — Pre-built agent system
- They're **layers, not competitors**. LangGraph is built on top of LangChain. Deep Agents is built on top of LangGraph.

## The Three-Layer Stack

1. **LangChain** (Framework) — Models, tools, prompts, chains, LCEL
2. **LangGraph** (Runtime) — State graphs, nodes, edges, checkpointing, HITL
3. **Deep Agents** (Harness) — Planning, filesystem, subagents, context engineering

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

## When to Use Deep Agents

- Complex, multi-step tasks that need planning and decomposition
- Large context management (files, scripts, artifacts)
- Long-running sessions (auto-summarization keeps agent effective)
- Coding agents (filesystem + shell + sandbox + planning)
- Autonomous subagent spawning for task delegation
- You want batteries-included without building a graph

## Deep Agents Core Capabilities

1. **Planning** — `write_todos` tool for task decomposition and progress tracking
2. **Filesystem** — `ls`, `read_file`, `write_file`, `edit_file` with pluggable backends
3. **Subagents** — Delegate work with context isolation
4. **Context Engineering** — Auto-summarization, large tool result eviction, long-term memory
5. **Shell Execution** — `execute` tool with sandbox backend
6. **Human-in-the-loop** — Built-in `interrupt_on` parameter

## LangGraph's 3 Primitives

1. **State** — typed dict that flows through the graph (not just messages, arbitrary data with reducers)
2. **Nodes** — functions that do work (read state, return state updates)
3. **Edges** — connections; unconditional or conditional (routing functions that examine state)

## Common Pitfalls

1. Don't use LangGraph for everything — simple chains don't need graph complexity
2. If your prompt has 5 paragraphs of conditional instructions, you've outgrown the agent loop — move control flow to code
3. LangGraph isn't just for multi-agent — single agents with complex workflows benefit too
4. Don't use Deep Agents for simple tasks — the overhead of planning tools and filesystem isn't worth it
5. Start simple, graduate up the stack when you feel the pain

## Decision Checklist

1. Need planning, file management, or autonomous subagents? → Deep Agents
2. Need loop/retry? → LangGraph
3. Need conditional branching? → LangGraph
4. Need human approval mid-execution? → LangGraph
5. Need state persistence across sessions? → LangGraph
6. Multi-agent system? → LangGraph
7. Linear pipeline or simple agent loop? → LangChain

## Architecture Patterns

- Simple RAG → LangChain LCEL
- Agentic RAG (re-query if irrelevant) → LangGraph
- Supervisor Agent (delegate to workers) → LangGraph or Deep Agents
- Simple Tool Agent (think → act → observe) → LangChain AgentExecutor
- Plan-and-Execute (decompose → execute → replan) → LangGraph
- Autonomous Research Agent (planning + files + context) → Deep Agents
- Coding Agent (filesystem + shell + sandbox) → Deep Agents
