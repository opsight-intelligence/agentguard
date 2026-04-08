# AgentGuard

> Claude Code has full access to your file system and shell.  
> It can read your `.env` files, run `rm -rf`, execute `git push --force`, and `curl` data to external servers.  
> AgentGuard stops it.

Three-layer enforcement — behavioral rules, permission denials, and deterministic hook scripts — that prevents AI coding agents from accessing secrets, executing dangerous commands, or leaking sensitive data.

Built by [Opsight Intelligence](https://opsightintel.com).

---

## Install in 2 minutes

```bash
git clone https://github.com/opsight-intelligence/agentguard.git
cd agentguard
./install.sh
```

Restart Claude Code. That's it.

**Prerequisites:** `jq` must be installed.
- macOS: `brew install jq`
- Ubuntu/Debian: `sudo apt install jq`

---

## What it blocks

| Category | Examples |
|---|---|
| **Sensitive files** | `.env`, credentials, SSL certs, SSH keys, cloud configs, secrets directories |
| **Dangerous commands** | `rm -rf`, `sudo`, `chmod 777`, `DROP TABLE`, `DELETE` without `WHERE`, pipe-to-shell |
| **Git operations** | All `git` commands — agent writes them as text, you run them manually |
| **Data exfiltration** | `curl`/`wget` uploads, `base64` of secrets, `/tmp` writes, `netcat` channels |
| **Untrusted packages** | `pip`/`npm`/`gem` from git URLs, custom registries, direct downloads |
| **Environment escape** | `ssh`, `docker run/exec/build`, `terraform apply/destroy`, `kubectl delete` |
| **PII in code** | SSNs, credit card numbers, Korean Resident Registration Numbers |

---

## How the three layers work

**Layer 1 — CLAUDE.md (behavioral)**  
18 security rules Claude Code reads at session start. Influences behavior but advisory only — layers 2 and 3 enforce deterministically.

**Layer 2 — settings.json (permission denials)**  
70+ hard deny rules in Claude Code's permission system. Blocks file access and command execution at the platform level before hooks run.

**Layer 3 — Hook scripts (deterministic)**  
8 bash scripts that intercept every tool call. Pattern-matched blocking with exit code 2. Full incident logging to `~/.claude/guardrail-blocks.log`.

Even if the AI ignores advisory rules, the deterministic layers will block prohibited actions.

---

## What gets installed

| File | Location | Purpose |
|---|---|---|
| `CLAUDE.md` | `~/.claude/CLAUDE.md` | Behavioral rules |
| `settings.json` | `~/.claude/settings.json` | Hard deny rules |
| `block-sensitive-files.sh` | `~/.claude/hooks/` | Credential and secret protection |
| `block-dangerous-commands.sh` | `~/.claude/hooks/` | Destructive command blocking |
| `block-git-commands.sh` | `~/.claude/hooks/` | Git operation blocking |
| `block-data-exfiltration.sh` | `~/.claude/hooks/` | Exfiltration prevention |
| `block-package-install.sh` | `~/.claude/hooks/` | Supply chain safety |
| `block-scope-escape.sh` | `~/.claude/hooks/` | Self-modification prevention |
| `block-environment-escape.sh` | `~/.claude/hooks/` | Environment isolation |
| `block-pii-leakage.sh` | `~/.claude/hooks/` | PII write prevention |

---

## Verify installation

```bash
./verify.sh
```

Checks that all hooks are present, unmodified, executable, and registered correctly.

---

## Update

```bash
cd agentguard
./update.sh
```

Pulls latest rules and re-installs. Merge strategy preserves any personal customisations.

---

## Customise

All rules are configurable without modifying Python:

- New file patterns → `claude/hooks/block-sensitive-files.sh` + `claude/settings.json`
- New dangerous commands → `claude/hooks/block-dangerous-commands.sh`
- New PII patterns → `claude/hooks/block-pii-leakage.sh`
- Behavioral rules → `claude/CLAUDE.md`

After changes: `./test.sh` to validate, `./install.sh` to deploy.

---

## Test suite

```bash
./test.sh
```

88 automated tests covering blocked and allowed cases for every hook.

---

## Troubleshooting

**Hooks not firing?**  
Run `/hooks` inside Claude Code to check registration. Verify scripts are executable: `ls -la ~/.claude/hooks/`

**`jq: command not found`?**  
Install jq — see Prerequisites above.

**Settings modified by developer?**  
Run `./verify.sh` to detect differences. Run `./install.sh` to restore.

---

## Community vs Pro vs Enterprise

| Feature | Community | Pro | Enterprise |
|---|---|---|---|
| 8 hook scripts | ✓ | ✓ | ✓ |
| 18 behavioral rules | ✓ | ✓ | ✓ |
| 70+ permission denials | ✓ | ✓ | ✓ |
| 3 slash commands | ✓ | ✓ | ✓ |
| Incident logging | ✓ | ✓ | ✓ |
| 88 automated tests | ✓ | ✓ | ✓ |
| CI/CD agents (GitHub Actions) | | ✓ | ✓ |
| LLM-powered code review (BYOK) | | ✓ | ✓ |
| Auto-fix on PR | | ✓ | ✓ |
| Vertical config packs (fintech, healthcare, legal) | | ✓ | ✓ |
| Multi-repo deployment | | | ✓ |
| Centralized incident dashboard | | | ✓ |
| Compliance reporting (ISO 27001, SOC2, HIPAA, PCI) | | | ✓ |
| Custom vertical development | | | ✓ |

[Contact us](mailto:utku@opsightintel.com) for Pro and Enterprise pricing.

---

## Questions or issues

Open an issue on GitHub or contact [utku@opsightintel.com](mailto:utku@opsightintel.com)

Built by [Opsight Intelligence](https://opsightintel.com) — fraud ecosystem intelligence and AI security for financial institutions.
