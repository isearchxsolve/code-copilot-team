---
feature_id: plugin-generate-from-sources
date: 2026-09-19
status: final
phase: build
mode: review
subject_provider: claude
peer_provider: deepseek
peer_profile: deepseek
runner_fingerprint: ae8dc147cf1264834bab35d116b6ab96b06618f438763cde3a90621d78036dba
verdict: PASS
blocking_findings_open: 0
target_ref: feat/363-plugin-generate-from-sources
rounds_completed: 1
attempt_count: 1
bypass: false
---

# Peer Review: plugin-generate-from-sources — Build Phase

**Reviewer**: deepseek
**Scope**: both
**Rounds**: 1
**Verdict**: PASS

## Summary


## Findings

- [note] f-d409d010: The rewrite regex `(~ (scripts/generate.sh)
- [note] f-66b2e758: The generated copy of `review-decide.sh` still carries the source's comment "installed by setup.sh" even though the plugin ships it under `${CLAUDE_PLUGIN_ROOT}/scripts/`. The origin-alignment record acknowledges this as a known limit, but a plugin-only user reading the script will be misled. (adapters/claude-code/plugin/scripts/review-decide.sh)
- [note] f-2477e725: The stale-file test creates `skills/zz-stale-skill/SKILL.md` and `commands/zz-stale-command.md`, then runs the generator and asserts removal. It does not assert that a stale file in `scripts/` or `templates/` is also removed, even though the generator cleans all five subdirectories. Coverage is slightly asymmetric. (tests/test-generate.sh)
- [note] f-8da29411: The agent references `${CLAUDE_PLUGIN_ROOT}/templates/sdd/hill-chart.json` as the schema source, which is correct for the plugin. However, the same agent's prose elsewhere (e.g., "cite from ${CLAUDE_PLUGIN_ROOT}/templates/sdd/hill-chart.json") is fine, but the surrounding text in the source adapter may still reference `~/.claude/templates/...` in a way the rewrite does not catch if the path lacks a trailing slash. Verified the rewrite covers the trailing-slash form; no action needed unless the source changes. (adapters/claude-code/plugin/agents/scope-executor.md)
- [note] f-84823f54: The doc states the plugin's copies of the five non-safety hooks "step aside when setup.sh's copies are installed, so formatting, verification and notifications run once." This is accurate, but it does not mention that the guard depends on the hook file being executable at `$HOME/.claude/hooks/<name>`. A user who installed setup.sh's hooks but later removed the executable bit would get double execution silently. (docs/install.md)