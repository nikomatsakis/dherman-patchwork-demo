# The AST

Threadbare programs are represented as JSON. There are three node types:

## Print

Outputs a message. The simplest node.

```json
{
  "Print": {
    "message": "Hello, world!"
  }
}
```

## Block

Executes a sequence of children in order.

```json
{
  "Block": {
    "children": [
      { "Print": { "message": "First" } },
      { "Print": { "message": "Second" } }
    ]
  }
}
```

## Think

The interesting one. Sends a prompt to an LLM and waits for a response. The LLM has access to a `do` tool that can execute any of the `children` subroutines.

```json
{
  "Think": {
    "think": {
      "prompt": "Pick a greeting. Call do(0) for formal, do(1) for casual.",
      "children": [
        { "Print": { "message": "Good morning, esteemed colleague." } },
        { "Print": { "message": "Hey!" } }
      ]
    }
  }
}
```

When the LLM calls `do { "number": 0 }`, the interpreter evaluates `children[0]` and returns the result to the LLM. The LLM can call `do` multiple times, or not at all.

## Composition

These nodes compose naturally. A Think's children can themselves contain Think nodes, enabling recursive LLM reasoning:

```json
{
  "Think": {
    "think": {
      "prompt": "Analyze this document. Call do(0) to get a summary.",
      "children": [
        {
          "Think": {
            "think": {
              "prompt": "Summarize: ...",
              "children": []
            }
          }
        }
      ]
    }
  }
}
```
