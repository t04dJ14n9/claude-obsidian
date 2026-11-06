# Prettier formatting

Formatting is opt-in per vault. Before writing Markdown pages, check the vault
root once per operation for a Prettier config: `.prettierrc`,
`.prettierrc.json`, `.prettierrc.yaml`, `.prettierrc.yml`, `.prettierrc.toml`,
`prettier.config.js`, `prettier.config.mjs`, or a `"prettier"` key in
`package.json`. Without one, do not format. Never change the vault's Prettier
config or ignore file unless the user asks.

Running `npx` may download Prettier from npm. Do it only when the user allows
network access to npm; otherwise skip formatting and say so.

## Format drafts

Format each Markdown draft before building the transaction bundle, from the
vault root so the vault's config and `.prettierignore` apply to the draft's
target path:

```bash
cd "$VAULT"
npx --yes prettier@latest --stdin-filepath "wiki/<page>.md" < draft.md > formatted.md
```

Record the formatted bytes' SHA-256 in the bundle. A page that
`.prettierignore` lists comes back unchanged.

- Format only `wiki/**/*.md`. Never format `.raw/` payloads, which are
  immutable, or ledgers, templates, Bases, and canvases.
- Format only the pages the operation writes. Reformatting other pages is a
  separate operation the user asks for.
- Prettier's Markdown parser misreads `_` and `*` next to CJK text as emphasis,
  and can nest consecutive footnote definitions. Write an in-word underscore in
  prose as `\_`. If Prettier still changes `_`, `*`, or footnote lines, keep the
  unformatted draft and tell the user the page belongs in `.prettierignore`.

## Check

After applying the operation, and in a lint run, check the vault from its root:

```bash
npx --yes prettier@latest --check "wiki/**/*.md"
```

Report the files it lists separately from the lint engine's findings; the
engine itself never runs Prettier. A new Prettier release can fail the check on
pages nobody changed: report them and leave them as they are.
