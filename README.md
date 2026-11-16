# python-kit

A kit for coding agents (Claude Code) working on Python projects: rules and hooks. Its sibling for
Swift is [swift-kit](https://github.com/macward/swift-kit); both share the manifest format and the
installer.

| Path | What it is | Installed as |
|---|---|---|
| `rules/python.md` | architecture, modules, typing, errors, async, pytest and style | `.claude/rules/python/`, loaded when Claude touches a `.py` file |
| `hooks/ruff-format-on-edit.sh` | runs `ruff format` on each Python file Claude edits | `.claude/hooks/`, wired in `settings.json` |
| `hooks/ruff-check-on-edit.sh` | hands Claude the `ruff check` violations of each edited file | `.claude/hooks/`, wired in `settings.json` |
| `hooks/block-dangerous-commands.sh` | refuses `rm -rf /`, `git push --force` and `git reset --hard` | `.claude/hooks/`, wired in `settings.json` |
| `manifest.json` | what the kit contains and how each hook is wired | read by `install.sh` and DeadCode's project setup |

The hooks and the installer need `jq`; the ruff hooks need `ruff` (`brew install jq ruff`). ruff
reads its settings from the project's `pyproject.toml`.

## Install in a project

```bash
git clone https://github.com/macward/python-kit.git /tmp/python-kit
bash /tmp/python-kit/install.sh path/to/project
```

The installer copies; it never symlinks. Commit the copies. The kit's files are overwritten on every
run and the hooks are merged into `.claude/settings.json` without duplicating them: to update, pull
the kit, run the installer again and review the diff. The copies are not edited in the project;
what is specific to it goes in its `AGENTS.md`.
