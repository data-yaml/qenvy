# QENV

## 1. Purpose

**QENV** gives Python developers a single, consistent place to store
everything that currently clutters individual repositories --- `.env`
files, build artifacts, temporary state, caches, and other per-developer
runtime data.

It is *not* an application framework. It is a **developer environment
layer**: a unified workspace that lives outside your repos and follows
the cross-platform directory rules of the **XDG Base Directory
Specification**.

## 2. What QENV Provides

1. A stable, cross-platform filesystem layout for your Python
   development tools
2. A single `config.toml` for settings that would otherwise be
   replicated per repo
3. Dedicated locations for shared cache, data, state, logs, and runtime
   artifacts
4. A structured, typed configuration loader (optional)
5. A predictable home for multi-repo tooling that previously relied on
   scattered `.env` files
6. A clean separation between version-controlled code and per-developer
   environment data

QENV replaces "every project has its own dotfiles" with one coherent,
portable environment.

## 3. Directory Structure

QENV exposes a central set of paths based on `platformdirs`. Everything
lives under the appropriate user-level directory:

- **Config**: long-lived user settings
- **Data**: shared persistent files used by tools
- **Cache**: temporary or regenerable build artifacts
- **State**: mutable history, logs, or working metadata
- **Runtime**: sockets, lock files, ephemeral process state

These directories are available via a single `EnvPaths` object.

## 4. Basic Usage

### 4.1 Load the Environment

```python
from qenv import EnvPaths, EnvConfig

paths = EnvPaths("qenv")

cfg = EnvConfig(
    paths=paths,
    defaults={"version": 1},
).load()
```

### 4.2 Access Common Locations

```python
paths.config_dir      # config.toml, user settings
paths.cache_dir       # shared build or dist artifacts
paths.data_dir        # small databases, indexes, etc.
paths.state_dir       # logs, history
paths.runtime_dir     # sockets, locks
```

## 5. Shared Tools, Shared Workspace

QENV is designed for Python developers working across multiple
repositories. Instead of each repo maintaining its own scattered
environment:

- A single `config.toml` stores user preferences
- Build and dist products accumulate cleanly in your cache directory
- Tools can interoperate through shared data or state
- Repositories stay uncluttered and version-control-friendly

Your development environment becomes consistent everywhere.

## 6. Optional Features

- **Profiles**: override `config.toml` with `config.<profile>.toml`
- **Schema validation**: plug in models (e.g., `pydantic`)
- **Version migrations**: evolve your config over time
- **Immutable results**: QENV returns an environment snapshot

These features stay lightweight and optional; QENV remains a thin layer.

## 7. Philosophy

QENV is a quiet piece of infrastructure:

- predictable
- unobtrusive
- cross-platform
- developer-centric
- repository-agnostic

You own your working environment, not each repo.
