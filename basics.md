---
title: Basics
subject: Tutorial
keywords:
  - phoenix
authors:
  - name: Ellyxir
    email: ellyse@ellyxir.com
---

## Nix Setup
Phoenix uses tailwindcss (maybe future me knows why people bother with this).
Phoenix adds a tailwindcss executable into the project but the executable is
dynamically linked which doesn't play well with Nix.
While there are better solutions, this is the lazy one. Add this to your Nix config:
```nix
  programs.nix-ld.enable = true;
  programs.nix-ld.libraries = with pkgs; [
    stdenv.cc.cc
    zlib
    openssl
  ];
```

Packages to add for filesystem watchers:
* `inotify-tools` - Phoenix docs say to install this
* `watchman` - used by tailwindcss engine

## Setup Application Generator
Install the Phoenix application generator `phx.new`:
```
$ mix archive.install hex phx_new
```

## Running Application Generator

Pass in the application name, and sqlite3 here because it's what we usually use:
```
$ mix phx.new <APP_NAME> --database sqlite3
```

:::{admonition} Command line options for the generator
* `--database <db>` overrides default (PostgreSQL) Ecto adapter
  * mysql - via https://github.com/elixir-ecto/myxql
  * mssql - via https://github.com/livehelpnow/tds
  * sqlite3 - via https://github.com/elixir-sqlite/ecto_sqlite3
* `--no-ecto` for no database
* `--no-live` if you don't want LiveView
* `--no-html` and `--no-assets` if youre not using HTML
:::

Now you'll want to get the deps and create the database:
```
$ mix deps.get
$ mix ecto.create
```

You should have a ready to run Phoenix app now. Let's run it:

```
$ mix phx.server
```

OR if you rather run it from within IEx:
```
$ iex -S mix phx.server
```

