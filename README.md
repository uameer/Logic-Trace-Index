# Logic-Trace (v0.1-alpha)
> An execution-aware index for agentic coding assistants.

## The Problem
AI coding assistants read files but are blind to runtime 
execution flow. The result: 50K+ tokens spent scanning 
code to understand a single login flow, with hallucinated 
fixes that break downstream dependencies.

## What This Is
A lightweight behavioral index (`.logic-trace/`) that maps 
how your application actually executes. Instead of reading 
code, the AI reads trajectories.

### Core Specification
- **Happy Path:** Compressed trace of function calls for core features
- **Error Surface:** Where the app historically fails based on log signatures
- **Side-Effect Map:** What happens after a database write

## Usage
Add this to your `CLAUDE.md` or `.cursorrules`:
