# Code repositories

A vault holds knowledge about code, never the code itself. Keeping every
checkout at a path derived from its remote URL lets an agent go from a page to
the code, and from the code back to the page and the right vault, without a
hand-kept mapping.

## Layout

Clone and manage code with [ghq](https://github.com/x-motemen/ghq) at its
default root. Ask `ghq root` for the root (normally `~/ghq`) instead of
hard-coding it. A checkout then lives at `<root>/<host>/<owner>/<repo>`.

- Clone: `ghq get <url>`. Find: `ghq list -p <query>`.
- Adopt an existing checkout: `ghq migrate --dry-run <dir>`, then without
  `--dry-run`. Leaving a symlink at the old path keeps scripts and editors that
  use it working.
- Worktrees: `git worktree add <repo>/.worktree/<branch>`, with `.worktree/` in
  the global Git ignore file. `ghq list` does not show worktrees; use
  `git worktree list`. `ghq migrate` (1.12) does not repair worktrees inside the
  moved checkout: afterwards run
  `git -C <new path> worktree repair <new path>/.worktree/<branch>` for each one.
- Perforce workspaces are not managed by `ghq`; keep them at
  `<p4-host>/<depot path>` under a separate root.
- If `ghq` is not installed, ask before installing it.

## Linking pages and code

- A page about a repository records its remote URL (for example a
  `repository_url` property). Record a local path only when the checkout is not
  at the `ghq` path of that URL.
- From a checkout, take `<host>/<owner>/<repo>` from its path under `ghq root`
  and search the vault for a page with that URL.

## Choosing the vault from a checkout

The core resolves the vault from the nearest `.claude-obsidian.json` above the
current directory. Put one at the `ghq` root that names the default vault, and
another in a host or group directory to send those repositories to a different
vault (for example a company Git host to a work vault):

```json
{"schema": "claude-obsidian.workspace.v1", "vault": "~/vaults/personal"}
```

Discovery from inside the claude-obsidian product checkout still fails closed,
even under such a config; pass `--vault` there.
