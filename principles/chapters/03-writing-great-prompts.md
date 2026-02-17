# Prompt engineering that actually works: examples, system prompts, and structure (Chapter 3)

## Summary

- Prompting skill is foundational: LLMs follow instructions well _if_ you specify them clearly.
- Core prompting modes: **zero-shot**, **single-shot**, **few-shot**; more examples usually improves control but costs tokens/time.
- A “seed crystal” tactic: ask the model to draft a v1 prompt, then iteratively refine.
- Use **system prompts** to shape assistant persona/tone (often more than accuracy).
- Formatting matters: capitalization, XML-like structure, and explicit sections (task/context/constraints) can improve reliability.
- The chapter includes an excerpt from a production code-generation system prompt (bolt.new) to show real-world prompt density.

## Prompting modes

### Zero-shot

Ask once, hope the model generalizes. Fastest, least control.

### Single-shot

Provide one example input/output pair to nudge behavior.

### Few-shot

Provide multiple examples to strongly steer output format and decisions.

- **Benefit:** precision and consistency
- **Cost:** more tokens, more authoring effort

## The “seed crystal” approach

When you’re stuck, ask the LLM to draft the prompt for you.

- “Generate a prompt for …”
- then: “Suggest improvements to this prompt”
  Often best results come from using the _same_ model you will later prompt (model-specific prompt style).

## System prompts

System prompts define “who the model is” and how it should behave.

- Great for **tone**, role, and guardrails
- Not guaranteed to increase accuracy on its own

## Formatting tricks that matter

- **CAPS** can weight importance
- **XML-like tags** can improve instruction following
- Structured sections: _Task / Context / Constraints / Output format_ tends to work well

## Example: production-grade code generation prompt excerpt

This excerpt illustrates “real prompts are long prompts”: lots of constraints, environment details, and explicit limitations.

```text
You are Bolt, an expert AI assistant and exceptional senior software developer with vast knowledge across multiple programming languages, frameworks, and best practices.

<system_constraints> You are operating in an environment called WebContainer, an in-browser Node.js runtime that emulates a Linux system to some degree. However, it runs in the browser and doesn't run a full-fledged Linux system and doesn't rely on a cloud VM to execute code. All code is executed in the browser. It does come with a shell that emulates zsh. The container cannot run native binaries since those cannot be executed in the browser. That means it can only execute code that is native to a browser including JS, WebAssembly, etc.

The shell comes with `python` and `python3` binaries, but they are LIMITED TO THE PYTHON STANDARD LIBRARY ONLY This means:

- There is NO `pip` support! If you attempt to use `pip`, you should explicitly state that it's not available.
- CRITICAL: Third-party libraries cannot be installed or imported.
- Even some standard library modules that require additional system dependencies (like `curses`) are not available.
- Only modules from the core Python standard library can be used.
Additionally, there is no `g++` or any C/C++ compiler available. WebContainer CANNOT run native binaries or compile C/C++ code!

Keep these limitations in mind when suggesting Python or C++ solutions and explicitly mention these constraints if relevant to the task at hand.

WebContainer has the ability to run a web server but requires to use an npm package (e.g., Vite, servor, serve, http-server) or use the Node.js APIs to implement a web server.
```

## Practical takeaways

- If output must be reliable, move from **zero-shot → few-shot**.
- For high-risk tasks (code, tools, compliance), use:
  - explicit constraints
  - explicit output schema
  - structured sections
- Treat prompt writing like programming: iterate, test, and measure (later chapters cover evals).
