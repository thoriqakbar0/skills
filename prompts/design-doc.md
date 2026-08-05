# design doc command

turn an implementation plan into a design document that supports a technical decision.

## usage

```bash
/design-doc                    # Start a new design doc
/design-doc "feature name"     # Start with a specific feature
```

## workflow

### 1. gather context

ask these questions:
- What problem are you solving?
- what is difficult about the current state?
- Any constraints or requirements?

Research the codebase if needed to understand existing patterns.

### 2. generate design doc

Output the design doc using this exact structure:

---

# {project name} design doc

## problem context

Brief description of the problem or opportunity. Overview of the domain and pain points. What is the current solution? What are its shortcomings?

## proposed solution

High-level summary of the proposed solution:
- What it will do
- How it will be built
- What's different from current state
- Key advantages

## goals and non-goals

### goals

- Goal 1: expected impact
- Goal 2: expected impact
- Goal 3: expected impact

### non-goals

- Non-goal 1 (explain why out of scope)
- Non-goal 2

## design

Overall summary of the design and major components.

```
[Include diagram if helpful - ASCII or mermaid]
```

### key components

Describe major request paths, data models, and architectural decisions.

Add subsections for each major component as needed:
- Component A
- Component B

## alternatives considered

| Alternative | Pros | Cons | Why Not Chosen |
|-------------|------|------|----------------|
| Option A | ... | ... | ... |
| Option B | ... | ... | ... |

## open questions

- [ ] Question 1
- [ ] Question 2

## implementation plan

### phase 1: foundation
- Task 1
- Task 2

### phase 2: core implementation
- Task 3
- Task 4

### phase 3: polish & testing
- Write tests
- Documentation

## appendix

Relevant links, detailed figures, or additional context.

---

### 3. iterate

After generating:
- Ask if any sections need expansion
- Clarify open questions
- Refine based on feedback

## rules

- use short, complete sentences.
- No fake case studies or made-up numbers
- Include realistic implementation phases
- Always include a testing phase
- List unresolved questions at the end
- Use tables for comparisons
- Include code snippets or diagrams where helpful

## output format

```
# {project name} design doc

[Full document as specified above]

---

Open questions to discuss:
1. ...
2. ...

Choose the next action: revise a section or start implementation.
```
