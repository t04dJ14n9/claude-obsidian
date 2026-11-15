# Internal link style

Before writing links to vault files, read `.obsidian/app.json` at the vault
root once per operation. Never change that file.

## Default: wikilinks

When the file is missing or unreadable, or `useMarkdownLinks` is not `true`,
link with wikilinks: `[[Note]]`, `[[Note|Label]]`, `[[Note#Heading]]`,
`[[Note#^block-id]]`, and `![[image.png]]`.

## Markdown links: `"useMarkdownLinks": true`

The vault is also read on GitHub and in VS Code, so write standard Markdown
links relative to the **vault root** (starting with `/`) that open the right
file in all three:

| Link to | Write |
|---|---|
| a note | `[Storage](/wiki/decisions/Storage.md)` |
| a heading | `[Use Postgres](/wiki/decisions/Storage.md#use-postgres)` |
| a same-page heading | `[Use Postgres](#use-postgres)` |
| a block | `[evidence](/wiki/sources/My%20Source.md#^evidence-block)` |
| an image | `![diagram](/wiki/assets/diagram.png)` |
| a PDF page | `[p. 3](/wiki/assets/papers/paper.pdf#page=3)` |
| a captured source | `` [`.raw/captured/<sha256>.md`](/.raw/captured/<sha256>.md) `` |

- Path: relative to the vault root with a leading `/`, with the file
  extension, and spaces encoded as `%20` (for example `/wiki/resources/notes/Note.md`
  or `/.raw/captured/<sha256>.md`). Same-page anchors stay `#heading-slug`.
- Lint: `"markdown_link_style"` in `.vault-meta/lint.json` decides how
  `claude-obsidian lint` resolves Markdown links. `"any"` (the default) matches
  them like Obsidian, with basename, alias, and suffix fallbacks. `"path"`
  resolves them strictly by filesystem path, relative to the note or from the
  vault root. `"vault-root"`, which suits this style, is as strict and also
  reports file-relative links such as `Note.md` or `../areas/Area.md`. Vaults
  that link directly into `.raw/` set `"allow_hidden_links": [".raw/**"]`.
- Heading: the GitHub slug of the heading as GitHub renders it (emphasis and
  HTML tags removed, code kept). Lowercase it, drop punctuation except `-` and
  `_`, and replace each space with `-`. A repeated
  slug gets `-1` on its second use, `-2` on its third. The link jumps to the
  heading on GitHub and in VS Code; Obsidian opens the note at the top.
- Blocks jump only in Obsidian; GitHub opens the note.
- Link to a note rather than embedding it; GitHub cannot show a note embed.
- Properties keep quoted wikilinks (`"[[Note]]"`) in both styles, because
  lint checks only wikilinks there.
- In a chat answer rather than a vault file, write paths from the vault root
  **without** the leading `/`: `wiki/resources/notes/Note.md`, or
  `[Note](wiki/resources/notes/Note.md#heading)`. Editor chat panels (Cursor,
  VS Code) resolve such a path from the open workspace folder, which is the
  vault, but read a leading `/` as the top of the disk and cannot open it.
  Links written into vault files keep the leading `/`.

Never convert a page's existing links as a side effect of another edit.
