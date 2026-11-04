# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Status (as of 2026-11-04)

PR #1 finished, refreshed, and licensed the guide. Nothing is in flight. `PLAN.md` lists the small jobs left, and they wait until Tom asks for them.

## What this repo is

Tom Dean's beginner guide to Multipass on Ubuntu: a `README.md` tutorial and a CC BY 4.0 `LICENSE`, with no code. From 2023 until PR #1 it had `PUT OUTPUT HERE` placeholders and an empty `minikube` section. PR #1 filled the placeholders and removed the section (Tom's call).

Related repos under `southsidedean`:

- `intro-to-multipass-macos` is the same guide for macOS. When a change applies to both, keep them consistent.
- `cka-d-cluster-builder-multipass` is the CKA/CKAD lab built on Multipass, still in progress. Both guides should link to it once it has a release.
- `southsidedean` is the profile repo. Its README lists this guide under "Guides and labs".

## Where the sample output came from

All of it was captured on 2026-11-04 from an x86_64 Ubuntu 24.04 host on the Multipass 1.17.0 snap, using a temporary instance called `touched-dhole`.

Some steps weren't run:

- The `multipass shell` example uses `multipass exec` output with a shell prompt added, because both print the same file.
- `sudo snap install multipass` and `sudo snap remove multipass` still have 2023 sample output, which is why the install sample says 1.11.0 and the version check says 1.17.0. Test these on a disposable machine. Removing the snap from a shared host would break whatever else runs on Multipass there.

## Testing

Every sample in the README comes from a real run. Never invent output or edit it to look newer.

- Ask Tom before launching an instance, because the test machines are shared with other work.
- Run `multipass list` first. Anything you didn't create belongs to someone else, so leave it alone.
- `multipass purge` permanently removes every deleted instance on the host, including ones you didn't create. Only run it when `multipass list` shows nothing else. Otherwise clean up with `multipass delete --purge <name>`.

Scripts have tripped on two things:

- `Launched: my-vm in zone1` means the instance is `my-vm`. A script that takes everything after `Launched: ` gets the wrong name, and its cleanup fails with the instance still running.
- `multipass exec` reads stdin, so inside a heredoc or `bash -s` it consumes the rest of the script. Over SSH, use `ssh -n` and `</dev/null`.

Confirm cleanup with `multipass list` afterward, since a script's exit status won't show a leftover instance.

## Conventions

- Never invent facts. The public email is tom@southsidedean.com. Review prose with the `humanizer` skill, and keep Tom's voice and the guide's structure.
- Work goes through a branch and a PR, and Tom merges. Ask before committing, pushing, or opening PRs, and check `git status` and the current branch first. This repo has no CI or branch rules, so nothing checks a PR automatically.
- Private details, such as test host names, go in `CLAUDE.local.md`, never in this file or `PLAN.md`. `.git/info/exclude` keeps `CLAUDE.local.md`, `.claude/`, `.mcp.json`, `.backups/`, and `.DS_Store` out of git.
- Use they/them for Tom.
