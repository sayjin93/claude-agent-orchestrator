# Orchestrator template for Claude Code

A drop-in `.claude` folder. Copy it into the root of any project so that it ends up at `<project>/.claude/`. Nothing else is required.

## What it sets up

| Piece | File | Effect |
|---|---|---|
| Orchestrator | `rules/orchestration.md` | Loaded automatically in every session in the project. Makes the main session (Claude Fable 5.1) design the system, write briefs, delegate, verify and integrate instead of coding directly. |
| Implementer | `agents/implementer.md` | Latest Opus (alias `opus`). Owns all production code changes. |
| Researcher | `agents/researcher.md` | Latest Sonnet (alias `sonnet`). Read-only codebase exploration plus web and docs search. |
| Tester | `agents/tester.md` | Latest Sonnet (alias `sonnet`). Writes and runs tests, edits test files only. |
| Entry point | `skills/orchestrate/SKILL.md` | `/orchestrate <request>` runs the whole loop explicitly. The rule also applies without it. |
| Settings | `settings.json` | Default model `claude-fable-5-1`, effort `high`, default permission mode `auto`. |

## Install into a project

1. Copy this folder to `<project>/.claude/`.
2. If the project already has a `.claude/settings.json`, merge the three keys from this one into it instead of replacing the file (`model`, `effortLevel`, `permissions.defaultMode`).
3. If the project already has `.claude/agents/`, `.claude/rules/` or `.claude/skills/`, the files here have distinct names and can sit next to the existing ones.
4. Start Claude Code in the project. Run `/agents` to confirm `implementer`, `researcher` and `tester` are listed with their models.

The folder works at the user level too: copying `agents/`, `rules/` and `skills/` into `~/.claude/` applies them to every project without copying per project.

## Day-to-day use

- Ask for work the way you normally would. Small edits and questions are handled directly. Multi-file or design-heavy work goes through the loop automatically.
- Type `/orchestrate <what you want built>` to force the full loop.

## Model versions and effort

- The subagents use the aliases `opus` and `sonnet`, so they move to the newest model of each tier automatically when Anthropic releases one. To pin a version instead, replace the alias with a full ID such as `claude-opus-5-5`. Full IDs work in the CLI even though the VS Code extension's schema flags them as unknown.
- The orchestrator model in `settings.json` is pinned to `claude-fable-5-1` because the CLI has no `fable` alias (checked on Claude Code 2.1.129). When a newer Fable ships, update that one string.
- Effort is `high` everywhere: `effortLevel` in `settings.json` for the orchestrator session and `effort: high` in each agent file for the subagents. Accepted values are `low`, `medium`, `high`, `xhigh` (and `max` for agents). Raise the orchestrator to `xhigh` for hard design work; lower the researcher to `medium` if search turns feel slow.
- Permission mode `auto` lets the session act without a prompt for most actions. Remove `permissions.defaultMode` from `settings.json` to go back to the normal prompt behaviour for a project.

## Cost note

Fable costs more per token than Opus (roughly 2.5x at the time of writing). The template keeps Fable's work to design, review and coordination, and pushes the bulk of the tokens (code writing, research, test runs) to the cheaper models.
