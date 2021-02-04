<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Neovim LSP Update](#neovim-lsp-update)
  - [Dependencies](#dependencies)
  - [Install](#install)
  - [Config](#config)
  - [Use](#use)
  - [Status](#status)
  - [Roadmap](#roadmap)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Neovim LSP Update

![Test](https://github.com/alexaandru/nvim-lspupdate/workflows/Test/badge.svg)
![Open issues](https://img.shields.io/github/issues/alexaandru/nvim-lspupdate.svg)
![License](https://img.shields.io/badge/License-MIT-blue.svg)

Updates installed (or auto installs if missing) LSP servers, that are already
configured in your `init.vim`.

It does NOT handle (auto)removals, i.e. if you had a LSP
server installed and then removed it's config, it will NOT
remove it. Only installs and/or updates are supported.

## Dependencies

- [Neovim HEAD/nightly](https://github.com/neovim/neovim/releases/tag/nightly) (v0.5 prerelease);
- [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig).

NOTE: your user must be able to perform installs of packages corresponding to the LSPs
you will be using. I.e. if you install `npm` based LSPs, then you must be able to
run `npm i -g ...` successfully, **WITHOUT** sudo. That is the best practice anyway,
you should in general install "global" packages (whether we talk about NodeJS, Ruby,
Python or whatever) under your user and not at system level.

Please refer to http://npm.github.io/installation-setup-docs/installing/a-note-on-permissions.html
for setup instructions for NodeJS (which covers many of the LSPs available). For
LSPs in other languages, please refer to their own documentation for installing,
and best practices for setting up the environment.

## Install

Add `alexaandru/nvim-lspupdate` plugin to your `init.vim`, using your favorite
package manager, e.g.:

```
packadd nvim-lspupdate
```

## Config

I wrote this plugin as I did NOT want to manage installs manually anymore,
therefore I made it so that it requires NO configuration. It should work
out of the box for the supported configurations ([see status](#status)).

You can however override any (or all) of the commands used (which you can
[see here](lua/lspupdate/config.lua#L85)) by defining a `g:lspupdate_commands`
dictionary in your `init.vim`, i.e.:

```VimL
let g:lspupdate_commands = {'pip': 'pip install -U %s'}
```

Any commands you define will be added to the existing commands (and override
them) so you only need to define the ones you actually want to modify, not the
whole table.

Please refer to the comments in `config.lua` above mentioned for the keys
and values (commands) to use in that dictionary.

## Use

See [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig#quickstart) on
how to setup the LSP servers' configuration. Once you have them configured
(or any time later, for updates), then run:

```
:LspUpdate
```

and it will install any missing LSP servers as well as update the existing ones,
where possible (see below).

Currently, the command takes no options. This may or may not change in the future.

Hint: use `:checkhealth lspconfig` before/after to verify that the LSPs were
installed.

## Status

Mix of **`beta`**, `pre-alpha` and <s>not (yet) supported</s>.

|**`Beta`**|`pre-Alpha`|<s>not supported</s>|
|:-:|:-:|:-:|
|**`angularls`**            |`clojure_lsp`       |<s>als</s>|
|**`bashls`**               |`dhall_lsp_server`  |<s>ccls</s>|
|**`cmake`**                |`flow`              |<s>clangd</s>|
|**`cssls`**                |`racket_langserver` |<s>codeqlls</s>|
|**`diagnosticls`**         |`rls`               |<s>dartls</s>|
|**`dockerls`**             |`rnix`              |<s>denols</s>|
|**`efm`**                  |`solargraph`        |<s>elixirls</s>|
|**`elmls`**                |`sorbet`            |<s>gdscript</s>|
|**`fortls`**               |                    |<s>ghcide</s>|
|**`gopls`**                |                    |<s>groovyls</s>|
|**`graphql`**              |                    |<s>hie</s>|
|**`html`**                 |                    |<s>hls</s>|
|**`intelephense`**         |                    |<s>jdtls</s>|
|**`jedi_language_server`** |                    |<s>julials</s>|
|**`jsonls`**               |                    |<s>kotlin_language_server</s>|
|**`leanls`**               |                    |<s>metals</s>|
|**`nimls`**                |                    |<s>ocamllsp</s>|
|**`ocamlls`**              |                    |<s>omnisharp</s>|
|**`purescriptls`**         |                    |<s>perlls</s>|
|**`pyls`**                 |                    |<s>pyls_ms</s>|
|**`rome`**                 |                    |<s>pyright</s>|
|**`sqlls`**                |                    |<s>r_language_server</s>|
|**`sqls`**                 |                    |<s>rust_analyzer</s>|
|**`svelte`**               |                    |<s>scry</s>|
|**`texlab`**               |                    |<s>sourcekit</s>|
|**`tsserver`**             |                    |<s>sumneko_lua</s>|
|**`vimls`**                |                    |<s>terraformls</s>|
|**`vuels`**                |                    |<s>vls</s>|
|**`yamlls`**               |                    |<s>zls</s>|

About half of the servers have [a config and a command](lua/lspupdate/config.lua)
defined. Of those, only `npm`, `pip`, `go` and `cargogit` commands were
tested. These are what I would consider beta.

The others that do have a config and a command defined, but were not yet
tested (`gem`, `cargo`, `nix`, etc.) I would consider pre-alpha. In theory,
they may work. If you do use them and they work, please let me know so I
can update this README accordingly.

Those that do not even have a config or command defined are, obviously,
not supported.

## Roadmap

In no particular order:

- dry run (show but don't actually run the commands);
- integration tests;
- add support for github binary releases;
- make the config configurable by end users (so they can override
  particular entries and use alternate sources/versions/etc.);
- add a nice way to handle "complicated installs" (yes sumenko_lua,
  that includes you);
- give a way to end users to control if the packages (for a
  particular LSP or package kind) are "merged" into a single
  command or handled individually;
- ensure it runs well on all of {Linux,MacOS,Windows};
- add a make target to verify our config against nvim-lspconfig
  to ensure we're catching any additions to the list of supported
  LSP servers.
