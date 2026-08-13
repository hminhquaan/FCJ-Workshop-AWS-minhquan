---
title: "Event 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Agent Forge – Deep Dive (Day 2): Memory, Evaluation, and Observability for AI Agents

### Purpose of the Event

The second session of the **AWS FCAJ Agent Forge – Deep Dive** workshop was organized by the First Cloud AI Journey (FCAJ) community together with AWS engineers. At an advanced (L300) level, the workshop focused on building AI Agents that can run in real enterprise environments.

The session was designed to help participants:

- Strengthen their understanding of memory management, response quality evaluation, system monitoring, and performance optimization.
- Get hands-on experience turning a basic AI Agent into a production-ready Agentic AI system.

### List of Speakers

- **Hieu** - Co-head of the FCAJ community, Solution Architect at AWS Vietnam.
- **Hai Anh** - Cloud Consultant at Chiase Pacific, leading the hands-on lab.
- **Nghia Tran** - Agentic AI Solution Architect.
- **Anh Pham** - Cloud Consultant at G-AsiaPacific Vietnam.

### Workshop Format

This was a **3-day workshop series** designed to take participants from the fundamentals all the way to deploying AI Agents in production using Amazon Bedrock AgentCore.

- **Day 1 (01/08): AgentCore Foundations** — overview of the AgentCore architecture (Runtime, Gateway, Identity).
- **Day 2 (08/08): Memory, Evaluations, Observability & Optimization** — managing memory, evaluating agent quality, monitoring the system, and tuning performance.
- **Day 3 (15/08): DevOps, Policies & Production Best Practices** — DevOps for agents, policies, security, and production best practices.

## Key Highlights

### Agent Memory

Agent Memory helps an Agent overcome the limits of the context window by keeping conversation context and personalizing the user experience.

**Short-term Memory** stores the full conversation history as raw messages, allowing the Agent to follow the current flow and respond consistently. The system also supports branching, similar to how Git creates branches.

**Long-term Memory** works asynchronously, extracting important information and storing it as vectors for later retrieval. The four main strategies are:

- **Summary:** summarize and compress the conversation.
- **User Preference:** store user preferences.
- **Semantic:** store domain knowledge.
- **Episodic:** record past decisions or events.

**Namespace** acts like a hierarchical folder structure to isolate data by strategy, actor, or session. Combined with semantic search and similarity ranking, the Agent can retrieve the right information while reducing token usage and improving response time.

### Observability

The workshop emphasized: *"You cannot fix what you cannot see."* Observability uses the OpenTelemetry standard to collect three types of data:

- **Logs:** record request details, connection errors, system errors, or terminal output.
- **Traces:** follow the full journey of a request from prompt to response, including tool calls.
- **Metrics:** measure token consumption, error rates, and response latency.

These help teams identify the cause of slowness, optimize token cost, and improve user experience.

### Agent Evaluation

A common risk of AI Agents is **hallucination** — producing inaccurate information that sounds confident. To reduce this, the system provides 13 built-in evaluators, such as **correctness** and **helpfulness**.

Evaluators are applied at three levels:

- **Session level:** evaluate the whole working session.
- **Trace level:** evaluate the accuracy of a response.
- **Span level:** evaluate each processing step, such as a tool call or parameter passing.

Two forms of evaluation are supported. **On-demand** fits the development and testing phase; **Online** monitors agent quality in real time in production. Automated evaluation results still need to be verified by domain experts.

## What I Learned

### Technical Knowledge

- Understood the difference between Short-term and Long-term Memory, especially the synchronous and asynchronous processing.
- Learned the three pillars of Observability: Logs, Traces, and Metrics, and the role of OpenTelemetry.
- Understood how automated evaluators assess agent responses against standardized criteria.
- Learned about Cedar Policy and sandbox mechanisms, and the importance of security when agents execute tasks or test code.

### Practical Lessons

- Design an AI Agent in small, focused functions before building a complex system.
- Always prioritize security and access control when an agent accesses resources.
- Monitor, evaluate, and optimize agents based on real results.
- Build agents that are easy to scale and maintain.

## Experience in the Workshop

Taking part in **Day 2 of the AWS FCAJ Agent Forge – Deep Dive** gave me a clear view of how to build and operate AI Agents in an enterprise environment.

Through the speakers' presentations and hands-on content, I better understood how to build an effective AI Agent by providing the system with knowledge storage, monitoring, quality evaluation, and strong security mechanisms.

### Photos from the event

![Event Photo 1](/images/4-EventParticipated/image001.jpg)

> **Overall assessment:** Day 2 of the **AWS FCAJ Agent Forge – Deep Dive** provided a solid foundation on **Agentic AI** and **Amazon Bedrock AgentCore**, helping participants go from basic concepts to building and deploying AI Agents in production. The workshop combined theory, examples, and hands-on practice, while emphasizing security, scalability, lifecycle management, and tool integration. It is a highly useful program for anyone who wants to build enterprise-ready AI Agent systems.
