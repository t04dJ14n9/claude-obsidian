# Vault instructions

This directory is a claude-obsidian knowledge vault. Agents that open it read
this file first; it is yours to extend with the vault's own rules.

## Working in this vault

- Use the claude-obsidian skills for vault work: `wiki` (setup and routing),
  `save`, `wiki-ingest`, `wiki-query`, and `wiki-lint`. They carry the current
  rules, so this file stays short.
- Change `wiki/` and `.raw/` only through the plugin core's reviewed
  transactions, never with direct file edits. Every change then stays
  recoverable and the provenance ledgers stay consistent.
- `inbox/` is intake for new material. Files in `.raw/` are captured sources
  and are never modified.
- Never invent citations, quotations, page numbers, or confidence.
- Code lives outside the vault; pages about a repository record its remote URL.
- Commit, push, or publish only when asked.

## Instruction files

Claude Code reads this file only while the vault has no `CLAUDE.md`. If you add
one, put `@AGENTS.md` in it so these rules still load.

## This vault's rules

Add vault-specific rules here.
