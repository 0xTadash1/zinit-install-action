# zpm-install

[![.github/workflows/test.yml](https://github.com/0xTadash1/zpm-install/actions/workflows/test.yml/badge.svg)](https://github.com/0xTadash1/zpm-install/actions/workflows/test.yml)

Install a zsh plugin manager on your Action

- [antibody](https://github.com/getantibody/antibody)
- [zinit](https://github.com/zdharma/zinit)

## NOTE

### `zsh`

This action only install zinit, not zsh. So, you need to have zsh installed and ready to use first.

E.g. One of the easiest ways to do that ([.github/workflows/test.yml#L7-L11](https://github.com/0xTadash1/zpm-install/blob/main/.github/workflows/test.yml#L7-L11))
:
```yaml
jobs:
  test_run:
    name: Test for action.yml
    runs-on: ubuntu-latest
    container: zshusers/zsh:5.8
 ```

 We can use `zshusers/zsh` via `container` syntax. Just one line! \
 Thanks to [zsh-users/zsh-docker](https://github.com/zsh-users/zsh-docker) and its maintainers!

### `git`, `subversion`, `curl`

You should have at least `git`, `subversion` and `curl` installed for installing zinit itself and some plugins.

E.g. ([.github/workflows/test.yml#L16-L20](https://github.com/0xTadash1/zpm-install/blob/main/.github/workflows/test.yml#L16-L20))
```yaml
    steps:
      - name: Packages update and install
        run: |
          apt update && apt upgrade -y
          apt install -y git subversion curl wget
```

### `source "${ZDOTDIR:-$HOME}/.zshrc"`

Even if you give `--rcs` option to `shell: ...`, `.zshrc` will not be loaded and you cannot use zinit. \
Therefore, you must `source` manually.

E.g. ([.github/workflows/test.yml#L33](https://github.com/0xTadash1/zpm-install/blob/main/.github/workflows/test.yml#L33))
```shell
          source "${ZDOTDIR:-$HOME}/.zshrc"
```

### `shell: ...`

In the step of executing zinit commands, `shell: ...` with the `-e/--errexit` flag may cause unexpected interruptions.

E.g. ([.github/workflows/test.yml#L45](https://github.com/0xTadash1/zpm-install/blob/main/.github/workflows/test.yml#L45))
```yaml
        shell: zsh -df --pipefail {0}
```

## Optional inputs

You can customize `$ZINIT[*]` array and `$ZPFX` via `with` syntax. \
By modifying this variables, you can control where zinit, plugins, and others are stored.

If nothing is specified, `${ZDOTDIR:-$HOME}/.zinit` will be used as the root by default.

Details: https://github.com/zdharma/zinit#customizing-paths

|input|variable|
|---|---|
|`zinit_home`|`$ZINIT[HOME_DIR]`|
|`zinit_bin`|`$ZINIT[BIN_DIR]`|
|`zinit_plugins`|`$ZINIT[PLUGINS_DIR]`|
|`zinit_completions`|`$ZINIT[COMPLETIONS_DIR]`|
|`zinit_snippets`|`$ZINIT[SNIPPETS_DIR]`|
|`zpfx`|`$ZPFX`|

## LICENSE

All code in this repository is released under the [MIT](https://github.com/0xTadash1/zpm-install/blob/main/LICENSE) license.
