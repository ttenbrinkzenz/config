# config

```
"$schema" = 'https://starship.rs/config-schema.json'

[directory]
truncation_length = 8
truncate_to_repo = false
before_repo_root_style = "bold cyan"
repo_root_style = "bold cyan"

[bun]
disabled = true

[cmake]
disabled = true

[nix_shell]
disabled = true

# Surface direnv state in the prompt. Necessary because direnv-instant
# runs the daemon asynchronously and doesn't print the usual
# "is blocked. Run `direnv allow` to approve its content" message
# synchronously the way plain direnv does.
#
# The built-in module reads direnv state directly. Good states
# (allowed_msg/loaded_msg) are set to "" so the format collapses to
# nothing via the `(...)` optional wrapper — only the problem states
# render in the prompt.
[direnv]
disabled = false
allowed_msg = ''
loaded_msg = ''
unloaded_msg = ''
denied_msg = '⚠ direnv: denied'
not_allowed_msg = '⚠ direnv: run `direnv allow`'
format = '([$allowed]($style) )'
style = 'bold yellow'

[custom.nix_dev]
description = "Nix dev shell — shows worktree name when PWD matches the flake the shell came from"
when = '[ -n "$IN_NIX_SHELL" ] && [ -n "$out" ]'
command = '''
  case "$out" in /nix/store/*) exit 0 ;; esac
  r=${out%/outputs/out}
  case "$PWD/" in "$r/"*) ;; *) exit 0 ;; esac
  p=${r%/*}
  printf 'shell=%s' "${p##*/}"
'''
format = 'via [❄️ $output](bold blue) '
shell = ["bash", "--noprofile", "--norc"]
```
