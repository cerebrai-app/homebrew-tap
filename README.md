# homebrew-tap

Homebrew casks for the cerebrai command line tools, built from
[cerebrai-app/go-cerebrai](https://github.com/cerebrai-app/go-cerebrai).
They work on macOS and Linux, on Apple silicon, Intel and arm64.

| Cask | What it is |
| --- | --- |
| [`brainctl`](Casks/brainctl.rb) | The brainctl command line interface |
| [`workctl`](Casks/workctl.rb) | The workctl command line interface |

## Install

Homebrew 6 only loads non-official taps that you have trusted
([Tap Trust](https://docs.brew.sh/Tap-Trust)). Installing a cask by its full name trusts
that one cask:

```sh
brew tap cerebrai-app/tap
brew install --cask cerebrai-app/tap/brainctl
brew install --cask cerebrai-app/tap/workctl
```

`brew tap cerebrai-app/tap` adds this repository. Homebrew finds it by the `homebrew-`
prefix in its name.

To trust the whole tap instead, including casks added to it later, so that you can use
the short names:

```sh
brew tap cerebrai-app/tap
brew trust cerebrai-app/tap
brew install --cask brainctl workctl
```

Check that it worked:

```sh
brainctl version
workctl version
```

## Update and uninstall

`brew upgrade` picks up new releases after `brew update`:

```sh
brew update
brew upgrade --cask brainctl workctl
```

To remove the tools, the trust and the tap:

```sh
brew uninstall --cask brainctl workctl
brew untrust cerebrai-app/tap
brew untap cerebrai-app/tap
```

`brew untrust cerebrai-app/tap` is only needed if you trusted the whole tap. For a single
cask, use `brew untrust --cask cerebrai-app/tap/brainctl`.

## Maintaining

GoReleaser writes the files in `Casks/` when go-cerebrai publishes a release (see
the `go-release-publish` action in [cerebrai-app/actions](https://github.com/cerebrai-app/actions)), so
don't edit them by hand. Change `.goreleaser.yaml` in go-cerebrai instead.

The binaries are not signed, so on macOS each cask removes the quarantine attribute after
installing, which keeps Gatekeeper from blocking them.

This repository's workflows call the shared actions in
[cerebrai-app/actions](https://github.com/cerebrai-app/actions) and run on the
`self-hosted` runners:

| Workflow | What it does |
| --- | --- |
| [`claude.yml`](.github/workflows/claude.yml) | Runs Claude Code when someone mentions `@claude` |
| [`claude-code-review.yml`](.github/workflows/claude-code-review.yml) | Reviews every PR with the `code-review` and ponytail plugins |
| [`renovate.yml`](.github/workflows/renovate.yml) | Weekly self-hosted Renovate, configured by [`renovate.json`](renovate.json) to keep pinned actions current |

They need these secrets, set at the repository or organization level:

- `CLAUDE_CODE_OAUTH_TOKEN` for the Claude workflows.
- `RENOVATE_APP_CLIENT_ID` and `RENOVATE_APP_PRIVATE_KEY` for Renovate. Until they exist,
  the job only prints a warning.
