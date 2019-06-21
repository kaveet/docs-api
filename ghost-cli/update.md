---
title: "update"
date: "2018-10-01"
meta_title: "Ghost-CLI commands: update - Documentation"
meta_description: "Find out how to update your Ghost version using the CLI tool. Read more in the official Ghost documentation."
next:
  url: "/api/ghost-cli/doctor/"
  title: "Doctor"
keywords:
    - cli
    - ghost
    - update
    - upgrade
sidebar: "ghost-cli"
---

The vast majority of Ghost upgrades are automated from a single command. Typically, we release a minor version of Ghost 1-2 times per week.

The Ghost-CLI has a single command for upgrades: `ghost update`.

## Arguments

```bash
# Update to a specific version (1.0.0 or higher)
ghost update [version]

# Update to version 2.15.0
ghost update 2.15.0

```

## Options

The `ghost update` command always runs `ghost doctor` first to check that your site will update correctly.

Run `ghost update --help` for more detailed information.

```bash
# If an upgrade goes wrong, use the rollback flag
ghost update --rollback

# Install and re-download the latest version of Ghost
ghost update --force

# Updates to the latest within v1
ghost update --v1

# Don't restart after upgrading
ghost update --no-restart

# Disable the automatic rollback on failure
ghost update --no-auto-rollback

# Upgrade Ghost from a specific zip (useful for testing or custom builds)
ghost update --zip path/to/file.zip

# Disable memory availability checks in ghost doctor
ghost update --no-check-mem

```

## Major upgrades

Every 12-18 months we release a [major version](https://docs.ghost.org/faq/major-versions-lts/) which breaks backwards compatibility and requires a more involved upgrade process, including backups and theme compatibility.

Use the relevant upgrade documentation as a guide to the necessary steps for a smooth upgrade experience:

* [Upgrading to Ghost 1.0](https://docs.ghost.org/faq/upgrade-to-ghost-1-0/)
* [Upgrading to Ghost 2.0](https://docs.ghost.org/faq/upgrade-to-ghost-2-0/)


## Debugging

If running `ghost update` gives an error, try using `ghost run` to debug the error.
