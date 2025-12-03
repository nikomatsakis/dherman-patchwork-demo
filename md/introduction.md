# Introduction

**Threadbare** is a minimal demo of the Patchwork execution model. It demonstrates how a program can interleave deterministic computation with non-deterministic LLM "thinking" - and crucially, how the LLM can call *back* into the interpreter to execute more code.

The name is a pun: it's a bare-threads implementation of Patchwork, held together by minimal threads.

## The Core Idea

Patchwork programs mix two kinds of computation:

1. **Deterministic blocks** - traditional code that always produces the same output
2. **Think blocks** - prompts sent to an LLM, whose output is non-deterministic

The interesting part: a Think block can invoke deterministic subroutines (via a `do` tool), and those subroutines might themselves contain Think blocks. This creates a recursive interplay between the interpreter and the LLM.

## Why This Matters

This execution model enables:

- **Auditability**: You can trace exactly what decisions the LLM made and why
- **Composition**: Deterministic scaffolding with LLM "escape hatches" where judgment is needed
- **Recursion**: LLM decisions can trigger further LLM decisions, nested arbitrarily deep

## What's in This Book

- **The AST** - The three node types: Print, Block, and Think
- **An Example** - A concrete program that categorizes documents
- **The Interpreter** - How the interpreter executes Think nodes, with recursive call tracing
- **The Agent** - How the agent manages concurrent LLM sessions and routes messages
