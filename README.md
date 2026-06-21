# End Session Checkpoint Skill

A plain [Agent Skills](https://agentskills.io/specification) skill for creating strong end-of-session checkpoints and handoff notes for coding work.

Use it when you want an agent to wrap up a session, inspect repo state, ask final handoff questions, optionally commit/push with approval, and write a restart note to:

```text
.agents/context/CURRENT_STATUS.md
```

## Repo layout

```text
end-session-checkpoint/
└── SKILL.md
```

The skill name is `end-session-checkpoint` and the parent directory matches the `name` field, as required by the Agent Skills spec.

## Install with GitHub CLI

GitHub CLI has `gh skill install` for installing Agent Skills into many agent hosts.

Preview before installing:

```bash
gh skill preview mosfor/end-session-checkpoint-skill end-session-checkpoint
```

Install for your default agent host and current project:

```bash
gh skill install mosfor/end-session-checkpoint-skill end-session-checkpoint
```

Pin the current release:

```bash
gh skill install mosfor/end-session-checkpoint-skill end-session-checkpoint --pin v0.1.0
```

Install for a specific agent at user scope:

```bash
gh skill install mosfor/end-session-checkpoint-skill end-session-checkpoint --agent AGENT --scope user
```

Examples:

```bash
# Claude Code
gh skill install mosfor/end-session-checkpoint-skill end-session-checkpoint --agent claude-code --scope user

# OpenAI Codex
gh skill install mosfor/end-session-checkpoint-skill end-session-checkpoint --agent codex --scope user

# GitHub Copilot
gh skill install mosfor/end-session-checkpoint-skill end-session-checkpoint --agent github-copilot --scope user

# Gemini CLI
gh skill install mosfor/end-session-checkpoint-skill end-session-checkpoint --agent gemini-cli --scope user

# Pi
gh skill install mosfor/end-session-checkpoint-skill end-session-checkpoint --agent pi --scope user
```

Supported `--agent` values in current GitHub CLI include:

```text
github-copilot, claude-code, cursor, codex, gemini-cli, antigravity,
adal, amp, augment, bob, cline, codebuddy, command-code, continue,
cortex, crush, deepagents, droid, firebender, goose, iflow-cli,
junie, kilo-code, kimi-cli, kiro-cli, kode, mcpjam, mistral-vibe,
mux, neovate, openclaw, opencode, openhands, pi, pochi, qoder,
qwen-code, replit, roo, trae, trae-cn, universal, warp, windsurf,
zencoder
```

This is the broadest install path. GitHub CLI owns the per-agent destination mapping, so users do not need to memorize every agent's skill directory.

Docs:

- GitHub CLI manual: <https://cli.github.com/manual/gh_skill_install>
- GitHub Copilot skills docs: <https://docs.github.com/en/enterprise-cloud@latest/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills>

## Manual install

If you do not want to use `gh skill`, copy the `end-session-checkpoint/` directory into your agent's skills directory.

```bash
tmpdir="$(mktemp -d)"
git clone https://github.com/mosfor/end-session-checkpoint-skill.git "$tmpdir/end-session-checkpoint-skill"
```

Then copy the skill directory to one of the locations below.

### Claude Code

Official docs: <https://code.claude.com/docs/en/skills.md>

User/global install:

```bash
mkdir -p ~/.claude/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" ~/.claude/skills/
```

Project install:

```bash
mkdir -p .claude/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" .claude/skills/
```

Invoke directly in Claude Code:

```text
/end-session-checkpoint
```

Or ask naturally:

```text
end this session and create a checkpoint
```

### OpenAI Codex

Official docs: <https://developers.openai.com/codex/skills>

User/global install:

```bash
mkdir -p ~/.agents/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" ~/.agents/skills/
```

Project install:

```bash
mkdir -p .agents/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" .agents/skills/
```

Codex can also install curated skills through `$skill-installer`; for this GitHub repo, `gh skill install` is the cleaner path.

### GitHub Copilot / VS Code / Copilot CLI / Copilot cloud agent

Official docs:

- GitHub Copilot: <https://docs.github.com/en/enterprise-cloud@latest/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills>
- VS Code: <https://code.visualstudio.com/docs/agent-customization/agent-skills>

User/global install:

```bash
mkdir -p ~/.copilot/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" ~/.copilot/skills/
```

Project install, GitHub-native location:

```bash
mkdir -p .github/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" .github/skills/
```

Project install, portable locations also supported by VS Code/Copilot docs:

```bash
mkdir -p .agents/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" .agents/skills/

mkdir -p .claude/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" .claude/skills/
```

### Pi coding agent

Official docs: <https://badlogic-pi-mono.mintlify.app/coding-agent/skills>

User/global install:

```bash
mkdir -p ~/.pi/agent/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" ~/.pi/agent/skills/
```

Portable global install shared with Codex and other agents:

```bash
mkdir -p ~/.agents/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" ~/.agents/skills/
```

Project install:

```bash
mkdir -p .pi/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" .pi/skills/
```

Or use `.agents/skills` in the project:

```bash
mkdir -p .agents/skills
cp -R "$tmpdir/end-session-checkpoint-skill/end-session-checkpoint" .agents/skills/
```

Invoke in Pi if skill commands are enabled:

```text
/skill:end-session-checkpoint
```

## Use

Ask naturally:

```text
end this session and create a checkpoint
wrap up this coding session
create a handoff note for the next agent
save current status before we stop
```

The skill tells the agent to:

- inspect git state
- ask a short exit interview
- commit or push only after explicit approval
- archive an old `.agents/context/CURRENT_STATUS.md`
- write a new restart-focused checkpoint

## Safety

Review any skill before installing it. Skills are instructions for your agent and can affect what tools it decides to run.

This skill tells the agent not to commit or push unless the user explicitly says yes.

## References checked

- Agent Skills specification: <https://agentskills.io/specification>
- Claude Code skills: <https://code.claude.com/docs/en/skills.md>
- OpenAI Codex skills: <https://developers.openai.com/codex/skills>
- GitHub Copilot skills: <https://docs.github.com/en/enterprise-cloud@latest/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills>
- VS Code Agent Skills: <https://code.visualstudio.com/docs/agent-customization/agent-skills>
- Pi skills: <https://badlogic-pi-mono.mintlify.app/coding-agent/skills>
- GitHub CLI `gh skill install`: <https://cli.github.com/manual/gh_skill_install>
