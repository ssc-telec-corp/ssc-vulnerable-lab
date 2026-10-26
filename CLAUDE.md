# CLAUDE.md — ssc-vulnerable-lab (PUBLIC, deliberately vulnerable)

## What this project is
A personal learning project in software supply chain security, run by an application security engineer (Product Security). Option A of the project plan: build a small demo service with a **deliberately flawed** GitHub Actions pipeline, attack it, then harden it. **This repo is the vulnerable half**; the hardened result lives in the public `ssc-hardened-pipeline` repo.

- Org: `ssc-telec-corp`. CI: GitHub Actions only.
- Ecosystems in scope: npm, PyPI, containers, NuGet. Azure is optional.
- Tooling constraint: open-source or free/free-trial tools only. No paid tools.
- Structure: 10 modules (Module 0 = planning/setup).
- Curriculum: https://claude.ai/artifact/N7Ebb3VtEvxCwD5fsbqL1N
- Module 0 spec: https://claude.ai/artifact/9iesvd42ngTg7hpM42qupa

## Lab scope
Intentional flaws (examples to be introduced per module, e.g. unpinned actions, over-broad `permissions`, untrusted input in `run:` steps, `pull_request_target` misuse, secret exposure, dependency confusion, unsigned artifacts). Each flaw should be documented: what it is, how it is exploited, how it was fixed in the public repo.

## Working mode: coach only
The user writes all code, pipelines and config. Claude Code sessions here MUST:
- Explain concepts, review diffs, and state what is still exploitable or unverified.
- NOT create, edit, commit or push files, and not open PRs, unless the user explicitly asks for that specific change.
- Before any action that changes something outside the chat, explain the intent and wait for approval.
- Not run attack steps against anything outside this lab repo and the user's own local test environments.

## Style
Documentary and precise. Separate facts from assumptions. When uncertain, say so, say how to verify it, and give conditions or ranges. Offer alternatives and residual risks. Each module needs a materials list (specs, docs, papers, incident write-ups, tools, links) stating which sources were actually opened and verified.

## Safety rules (private lab, deliberately vulnerable)
- **Visibility changed: this repo is now PUBLIC (originally planned as private).** Fact: anyone can open fork PRs, issues and comments against it. Assumption (verify in Settings > Actions): whether fork-PR workflows run automatically, and with what token permissions, depends on repo/org settings. Risk: flawed triggers (`pull_request_target`, `issue_comment`, script injection) can be exercised by third parties, not only by the owner, and any secret or credential reachable by those workflows can be exfiltrated.
- Mitigations, in order of strength: (1) switch back to private; (2) keep public but disable Actions, or set fork-PR workflows to require approval for all outside contributors, until a module needs them; (3) set default `GITHUB_TOKEN` to read-only; (4) no repo/org secrets, no cloud (Azure) credentials, no self-hosted runners, no environments with secrets; (5) enable only workflow triggers the current module needs, and re-check after each change.
- No real secrets. Use obviously fake canary values; if a real credential is ever committed, rotate it immediately and treat it as compromised.
- Fake "internal" package names (for dependency-confusion exercises) live only on local feeds (local registry/mirror). Never publish them to npmjs, PyPI, NuGet.org or any public registry; before any publish step, check the target registry and name.
- Because the repo is public, treat every flaw as live and internet-reachable; do not introduce a flaw that can reach anything beyond this repo.
- Never copy vulnerable workflows into the public repo; the public repo receives only hardened versions.
- Do not enable self-hosted runners on this repo unless isolated, since attacker-controlled workflow code could execute on them.
- Exploit demos target only this repo and the user's local feeds.

## Open uncertainties (verify, do not assume)
- Exact exploitability of each flaw depends on repo/org settings (default `GITHUB_TOKEN` permissions, fork-PR approval policy). Verify settings before concluding a flaw is or is not exploitable.
- Whether GitHub artifact attestations alone reach SLSA Build L3 is open; this matters for the hardening comparison, not for this repo.
- Free-tier limits for private repos (Actions minutes, available security features) are unverified until tested.

## Notes workflow
After each module the user takes notes and findings back to the Claude planning thread, which produces the per-module summary artifact. Mark anything unverified as such. Each module has its own thread.
