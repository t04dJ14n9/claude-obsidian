# Internal link style

Before writing links to vault files, read `.obsidian/app.json` at the vault
root once per operation. Never change that file.

## Default: wikilinks

When the file is missing or unreadable, or `useMarkdownLinks` is not `true`,
link with wikilinks: `[[Note]]`, `[[Note|Label]]`, `[[Note#Heading]]`,
`[[Note#^block-id]]`, and `![[image.png]]`.

## Markdown links: `"useMarkdownLinks": true`

The vault is also read on GitHub or in VS Code, so write standard Markdown
links that open the right file in all three:

| Link to | Write |
|---|---|
| a note | `[Storage](../decisions/Storage.md)` |
| a heading | `[Use Postgres](../decisions/Storage.md#use-postgres)` |
| a block | `[evidence](My%20Source.md#^evidence-block)` |
| an image | `![diagram](../assets/diagram.png)` |
| a PDF page | `[p. 3](../sources/paper.pdf#page=3)` |

- Path: relative to the note being written, with the file extension, and
  spaces encoded as `%20`.
- Heading: the GitHub slug of the visible heading text. Lowercase it, drop
  punctuation except `-` and `_`, and replace each space with `-`. A repeated
  slug gets `-1` on its second use, `-2` on its third. The link jumps to the
  heading on GitHub and in VS Code; Obsidian opens the note at the top.
- Blocks jump only in Obsidian; GitHub opens the note.
- Link to a note rather than embedding it; GitHub cannot show a note embed.
- Properties keep quoted wikilinks (`"[[Note]]"`) in both styles, because
  lint checks only wikilinks there.
- In a chat answer rather than a vault file, write paths from the vault root.

Never convert a page's existing links as a side effect of another edit.
