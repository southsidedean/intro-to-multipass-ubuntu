# intro-to-multipass-ubuntu plan

Tom Dean, updated 2026-11-04

The guide is complete and matches Multipass 1.17.0, apart from the install and uninstall samples noted below. Nothing here is urgent, and each item is its own small PR.

## Done

PR #1 (2026-11-04) replaced all 8 output placeholders with real output, added the `multipass find` step to match the macOS guide, removed the empty `minikube` section, replaced the HyperKit and VirtualBox text, moved the `multipass.run` links, fixed the references list, updated the tags, and added the CC BY 4.0 license.

## To do

1. Review the prose added on 2026-11-04 with the `humanizer` skill: the `lts` alias note, the zone wording, the `exit` hint, and the License section.
2. Check the steps that weren't run. Open a real `multipass shell` session and confirm the prompt and output. Rerun `sudo snap install multipass` and `sudo snap remove multipass` on a disposable Ubuntu machine and replace the 2023 samples.
3. Once `cka-d-cluster-builder-multipass` has a release, link to it from the References and Summary as the next step. Do the same in the macOS guide.

## Decisions

| Question | Decision |
| --- | --- |
| Keep or archive | Keep and finish (Tom, 2026-11-04) |
| `minikube` section | Removed: never written, and it relied on a deprecated blueprint (Tom, 2026-11-04) |
| License | CC BY 4.0, because the repo is prose only |
| Sample output | Real runs only |
