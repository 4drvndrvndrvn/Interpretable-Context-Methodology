# Interpreted-Context-Methdology

ICM is a framework for building structured, multi-stage AI workflows out of markdown files and folder conventions. Each workspace gives AI agents the right context at each stage of a task, and gives humans clear edit surfaces between stages.

## Folder Map

```
model-workspace-protocol/
├── CLAUDE.md                          (you are here)
├── README.md                          (project overview)
├── LICENSE
├── _core/                             (shared conventions and templates)
│   ├── CONVENTIONS.md                 (source of truth for all patterns)
│   ├── placeholder-syntax.md          (how {{VARIABLES}} work)
│   └── templates/                     (blank starting points for new workspaces)
└── workspaces/
    ├── script-to-animation/           (content idea -> animated video)
    ├── course-deck-production/        (unstructured material -> course PowerPoints)
    └── workspace-builder/             (builds new MWP workspaces)
```

## Routing

| You want to... | Go to |
|-----------------|-------|
| Create content with script-to-animation | `workspaces/script-to-animation/CLAUDE.md` |
| Build course slide decks from source material | `workspaces/course-deck-production/CLAUDE.md` |
| Build a new workspace for any domain | `workspaces/workspace-builder/CLAUDE.md` |
| Read the full MWP specification | `_core/CONVENTIONS.md` |
| Understand the placeholder system | `_core/placeholder-syntax.md` |
| Use a template for a new workspace | `_core/templates/` |

## Triggers

| Keyword | Action |
|---------|--------|
| `setup` | Run onboarding in whatever workspace you are in |
| `status` | Show pipeline completion for the current workspace |

## How It Works

Each workspace is self-contained with its own CLAUDE.md. Navigate into a workspace folder and that workspace's CLAUDE.md takes over. You do not need to read this root file once you are inside a workspace.

## Behavioral Guidelines

Behavioral guidelines to reduce common LLM coding mistakes, from [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/CLAUDE.md) (MIT), derived from [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls. They apply in every workspace, alongside that workspace's own CLAUDE.md.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

### 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

### 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

### 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

### 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
