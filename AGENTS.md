# claude-obsidian: Agent Instructions

claude-obsidian is a local-first Agent Skills package for building
source-cited, compounding Obsidian knowledge bases. It also ships a Claude Code
plugin adapter.

This checkout is the fork https://github.com/t04dJ14n9/claude-obsidian of
https://github.com/AgriciDaniel/claude-obsidian. Fork-only behavior is listed
under `[Unreleased]` and fork releases in `CHANGELOG.md`; keep it there so it
can be proposed upstream or dropped deliberately.
The portable workflow is implemented in `skills/` and the standard-library
`claude_obsidian/` core; host hooks never define knowledge behavior.

## Product and vault boundaries

- This repository is the product source. It is not the default user vault.
- A user vault is the directory containing `.claude-obsidian.json`, `wiki/`,
  and `.raw/`. Mutable state always belongs there.
- `templates/vault/` is the distributable seed. Root `wiki/`, `.raw/`, and
  `.vault-meta/` are contributor state and are excluded from public artifacts.
- Never derive a user vault from the plugin cache or `${CLAUDE_PLUGIN_ROOT}`.
- A checkout containing contributor-vault state has no marketplace catalog.
  `config/public-marketplace.json` is injected as
  `.claude-plugin/marketplace.json` only inside the audited release artifact.
  An extracted distribution-clean artifact may retain that exact manifest and
  rebuild idempotently. A public default branch must be populated from the clean
  artifact, never by pushing contributor-vault state.

Resolve a vault in this order: explicit `--vault`,
`CLAUDE_OBSIDIAN_VAULT`, nearest `.claude-obsidian.json`, then an unambiguous
vault at or above the current directory. Fail closed when no vault is selected.
Implicit discovery from inside the product tree always fails, even under a
`.claude-obsidian.json` in a parent directory (such as a `ghq` root); pass
`--vault` when running the core from this checkout.

## Bootstrap

1. Read this file. In a development checkout, also read the host-only root
   `CLAUDE.md` when present; release artifacts intentionally omit it.
2. Read the selected skill completely.
3. Read only the references that skill routes to.
4. Resolve the user vault. If `wiki/hot.md` exists, read it silently.
5. For a new vault, run `python3 "$PRODUCT_ROOT/scripts/claude-obsidian.py"
   init PATH` first, with `PRODUCT_ROOT` resolved from the skill's own location,
   never from the vault; apply only after reviewing the dry run. Use `adopt`
   for an existing vault. Both seed a starter `AGENTS.md` and never replace an
   existing one.

## Canonical skills

All 15 skills live at `skills/<name>/SKILL.md`. They use the portable Agent
Skills frontmatter subset: exactly `name` and `description`. Do not add mirrored
files under `commands/`; Claude invokes plugin skills by namespaced names such
as `/claude-obsidian:wiki`.

Core workflows are `wiki`, `save`, `wiki-ingest`, `wiki-query`, and
`wiki-lint`. Extensions are `autoresearch`, `canvas`, `defuddle`, `wiki-fold`,
`wiki-mode`, `wiki-retrieve`, and `wiki-cli`. Reference skills are
`obsidian-markdown`, `obsidian-bases`, and `think`.

## Mutation protocol

One logical knowledge operation is one recoverable transaction:

1. Read targets and record expected SHA-256 values.
2. Let parallel workers return drafts and evidence only.
3. Merge drafts into one `claude-obsidian.transaction.v1` bundle.
4. Inspect the bundle, then apply it once through `scripts/claude-obsidian.py`.
5. Report the operation ID and exact changed paths.

Do not use direct shared writes, the deprecated `wiki-lock.sh` helper, or
generic lifecycle auto-commits. Git checkpointing is separate and explicit.
Raw source payloads are create-only; `.raw/.manifest.json` is the only mutable
legacy raw metadata file. Destructive repairs, remote egress, and canonical
research merges require explicit consent.

## Vault conventions

- `inbox/`: visible capture intake; never deleted automatically.
- `.raw/`: immutable source payloads and legacy delta manifest.
- `wiki/`: generated knowledge pages.
- `wiki/meta/ledgers/`: source and claim provenance.
- `wiki/hot.md`: bounded recent context, never a transcript.
- `wiki/log.md`: operation history, newest first.
- `.vault-meta/`: ignored runtime locks, journals, indexes, queues, and config.

Use Obsidian Flavored Markdown: flat YAML properties, `YYYY-MM-DD` dates,
wikilinks (vault-root-relative Markdown links when the vault sets
`useMarkdownLinks`), embeds, and valid callouts. Never fabricate evidence
locators, quotations, page numbers, or confidence. Code checkouts live outside
vaults; see `skills/wiki/references/code-repositories.md`.

## Verification

Run `make test` after behavioral changes. It executes every Python and shell
suite plus the product, capability, package, hook, and manifest contracts.
Public artifacts are built locally with `release build` and audited without
publishing. No agent may push, tag, open or mutate issues, or publish a release
without explicit owner approval.

Claude SessionStart context injection is disabled by default. Treat
`CLAUDE_OBSIDIAN_SESSION_CONTEXT=1` as explicit user consent to place bounded
`wiki/hot.md` data in the model context; never set it automatically. A
workspace-configured vault outside the project also requires an exact
`CLAUDE_OBSIDIAN_SESSION_CONTEXT_VAULT` path.

## Reference

- Upstream repository: https://github.com/AgriciDaniel/claude-obsidian
- This fork: https://github.com/t04dJ14n9/claude-obsidian
- LLM Wiki pattern:
  https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- Obsidian primitives: https://github.com/kepano/obsidian-skills
