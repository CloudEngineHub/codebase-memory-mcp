# Maintainers

This document separates advisory review responsibility from binding merge
authority.

codebase-memory-mcp is currently a user-owned repository. Because GitHub teams
are not available here, all delegated ownership is expressed with individual
GitHub handles.

## Authority Model

| Role | Scope | Authority |
| --- | --- | --- |
| Project owner | Entire repository | Final roadmap, security, release, workflow, and merge authority. |
| Release operator | Dry-run and release preparation | May prepare release notes, run checklists, and request dry runs when explicitly delegated. May not publish without owner approval. |
| Area reviewer | One technical area | May review, test, reproduce, and recommend merge for that area. Approval is advisory until the project owner approves. |
| Triage collaborator | Issues and discussions | May label, deduplicate, reproduce, and request information. No merge or release authority. |

The binding rule is intentionally simple: all pull requests require
`@DeusData` approval before merge. `MAINTAINERS.md` routes review; it does not
override `.github/CODEOWNERS`.

## Project Owner

| Handle | Responsibilities |
| --- | --- |
| `@DeusData` | Final authority for merge decisions, releases, security handling, CI policy, repository settings, and maintainer promotion. |

## Area Review Map

Use this map to request informed review. Entries marked `TBD` are open slots
for future co-maintainers.

| Area | Paths | Advisory reviewers | Owner gate |
| --- | --- | --- | --- |
| MCP protocol and tool surface | `src/mcp/`, `tests/test_mcp.c` | TBD | `@DeusData` |
| CLI, install, update, editor integration | `src/cli/`, `src/git/`, `install.sh`, `install.ps1`, `tests/test_cli.c` | TBD | `@DeusData` |
| Indexing pipeline and graph construction | `src/pipeline/`, `src/discover/`, `src/watcher/`, `tests/test_pipeline.c`, `tests/test_incremental.c` | TBD | `@DeusData` |
| Language extraction and LSP resolution | `internal/cbm/`, `tools/`, `tests/test_*_lsp.c`, `tests/test_extraction.c`, `tests/test_grammar_*.c` | TBD | `@DeusData` |
| Store, query, and graph buffers | `src/store/`, `src/cypher/`, `src/graph_buffer/`, `src/simhash/`, `src/semantic/`, `tests/test_store_*.c`, `tests/test_cypher.c` | TBD | `@DeusData` |
| Foundation/runtime portability | `src/foundation/`, `vendored/`, `tests/test_*` foundation coverage | TBD | `@DeusData` |
| Graph UI backend and frontend | `src/ui/`, `graph-ui/`, `tests/test_ui.c`, `tests/test_httpd.c` | TBD | `@DeusData` |
| Tests, repro, smoke, and soak infrastructure | `tests/`, `tests/repro/`, `test-infrastructure/`, `scripts/test.sh`, `scripts/smoke-test.sh`, `scripts/soak-test.sh` | TBD | `@DeusData` |
| Packaging and distribution | `pkg/`, `install.sh`, `install.ps1`, `server.json`, `glama.json`, `flake.nix`, `flake.lock`, `THIRD_PARTY.md`, `scripts/gen-third-party-notices.sh`, `scripts/gen-ui-licenses.py`, release archive contents | TBD | `@DeusData` |
| Security and supply chain | `SECURITY.md`, `docs/SECURITY-DISCLOSURE.md`, `scripts/security-*`, `scripts/*license*`, `scripts/*allowlist*`, `.github/workflows/codeql.yml`, `.github/workflows/scorecard.yml` | `@DeusData` only initially | `@DeusData` |
| CI and release operations | `.github/workflows/`, `scripts/ci/`, `Makefile.cbm` | `@DeusData` only initially | `@DeusData` |
| Governance and contribution policy | `.github/CODEOWNERS`, `MAINTAINERS.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `DCO`, `LICENSE`, `.github/pull_request_template.md`, issue templates | `@DeusData` only initially | `@DeusData` |

## Operational Authority

Operational authority is stricter than code review authority.

| Operation | Current authority | Delegation rule |
| --- | --- | --- |
| PR validation (`pr.yml`, DCO, CodeQL) | Automatic | Anyone may trigger it by opening or updating a PR. Required checks must pass. |
| Dry run (`dry-run.yml`) | `@DeusData` | May be delegated to release operators after trust is established. Dry-run delegation does not imply release authority. |
| Smoke/soak/repro manual runs | `@DeusData` | May be delegated to area reviewers for diagnosis, but results are advisory. |
| Release workflow (`release.yml`) | `@DeusData` only | Owner-only until a release operator is explicitly promoted. Publishing, replacing releases, and tag movement remain owner-gated. |
| Package registry publishing | `@DeusData` only | Owner-only initially because registry credentials and public packages are irreversible operational surfaces. |
| Security advisory handling | `@DeusData` only | Do not delegate across advisories. Keep private reports isolated. |
| Workflow, ruleset, CODEOWNERS, and branch protection changes | `@DeusData` only | Owner-only because these define authority itself. |

Before granting a collaborator `Write` access, verify that they should be able
to run manual GitHub Actions workflows in this user-owned repository. Prefer
`Triage` for issue-only delegation. If `Write` becomes necessary, use protected
branches, required code-owner review, and protected release environments where
available so release jobs still require `@DeusData` approval.

## Promotion Path

Promotion is gradual and reversible.

| Stage | Requirements | Capabilities |
| --- | --- | --- |
| Trusted contributor | Focused PRs, clean DCO, responsive review cycles. | No elevated access. |
| Triage collaborator | Consistent issue reproduction and respectful scope control. | Issue triage and review requests. |
| Area reviewer | Sustained, technically accurate reviews in one area. | Advisory review listing in this file. |
| Release operator | Proven reliability on dry runs, packaging, and release checklists. | May run delegated dry runs and prepare releases. |
| Full maintainer | Long-term trust across code, security, release, and governance. | Only after explicit owner decision and updated policy. |

## High-Risk Change Rules

These changes always require project-owner review, even if an area reviewer
approves:

- MCP tool behavior, tool outputs, or protocol capabilities.
- New process execution, shell invocation, network access, or file access.
- CI, release, package publishing, installer, or updater changes.
- Vendored dependency changes, generated grammar changes, or license policy.
- Security disclosure process, advisories, allowlists, and audit scripts.
- Repository governance, branch protection, CODEOWNERS, or maintainer roles.

## Review Expectations

- Keep PRs scoped to one issue or one logical change.
- Reproduce bugs with a failing test before fixing.
- Treat area-reviewer approval as a technical recommendation, not final merge
  authority.
- Do not self-approve high-risk changes.
- Resolve conflicts of interest by asking another reviewer and the owner.
