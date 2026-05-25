# dual-research

Universal web research plugin for Claude Code with dual-LLM triangulation.

Claude researches independently, GPT-5 cross-checks via Codex CLI, findings are merged with conflict resolution and primary-source verification.

## Usage

```
/dual-research:research <topic or question>
```

Examples:
```
/dual-research:research What is the current state of account abstraction on Ethereum?
/dual-research:research Compare Bun vs Deno for production backend services
/dual-research:research Latest developments in WebAssembly component model
```

## Requirements

- Claude Code with WebSearch and WebFetch tools enabled
- **Optional**: [Codex CLI](https://github.com/openai/codex) for dual-LLM mode (`npm install -g @openai/codex`)

## Operating modes

### Dual mode (Claude + GPT-5)

When Codex CLI is installed and authenticated, the plugin spawns GPT-5 as a background cross-checker while Claude conducts its own research. After both complete independently (anti-anchoring), findings are merged with 4-rule verification:

- **Rule A**: Fresh-context verification for conflicts
- **Rule B**: Empirical CLI checks for software claims
- **Rule C**: Mechanical URL validation for cross-checker citations
- **Rule D**: Evidence-weight classification (evidence > vote count)

### Solo mode (Claude dual-pass)

When Codex CLI is unavailable, Claude conducts two independent research passes with different search angles, then merges findings using the same verification rules. Honestly disclosed as weaker due to shared model biases.

## Output

Structured report with:
- Executive summary
- Confidence map (claim / evidence level / sources / confidence)
- Detailed findings by sub-topic
- Agreements, additions, conflicts resolved
- Rejected claims (transparency)
- Open questions
- Synthesis value metrics
- Security notes

## Security

- Secure tempfiles via `mktemp` (kernel CSPRNG, not predictable timestamps)
- Model echo verification on cross-checker output
- All cross-checker output treated as untrusted
- Read-only sandbox for Codex (`-s read-only`, kernel-enforced)
- Prompt injection defense rules
- Automatic tempfile cleanup

## Installation

```bash
mkdir -p ~/.claude/plugins/local/plugins
cd ~/.claude/plugins/local/plugins
git clone <this-repo> dual-research
```

Or manually copy the plugin files to `~/.claude/plugins/local/plugins/dual-research/`.

Restart Claude Code after installation.

## Structure

```
dual-research/
├── .claude-plugin/plugin.json     # Plugin manifest
├── agents/researcher.md           # Research subagent
├── commands/research/COMMAND.md   # Main research orchestrator
└── README.md
```

## License

MIT
