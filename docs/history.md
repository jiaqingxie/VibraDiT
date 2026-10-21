# Version history

## Current: one-step VibraDiT

The default branch contains the one-step implementation, current configurations,
results and five main figures. The architecture, parameter keys, loss definitions,
data normalization, deterministic batches and validation selection come from the
completed October 20 campaign. Source-file hashes are recorded in
[`source_provenance.json`](source_provenance.json).

This reorganization removes obsolete experiment launchers, duplicate figure
drafts, manuscript build files and unrelated research modules from the default
tree. It preserves the Git history and does not delete or relocate the original
local experiment directories, datasets, checkpoints or manuscript worktree.

## Archived: multi-step SpectraFlow

The previous GitHub tree is preserved at tag
[`archive/spectraflow-20261003`](https://github.com/jiaqingxie/VibraDiT/tree/archive/spectraflow-20261003),
commit `88e29a72f8469123143fbd5f5fc8af01326352f1`.
It includes the earlier model implementations, raw-data preparation utilities,
manuscript snapshot and its reproduction audit.

The archived scores and checkpoint definitions belong to that version. They
should not be mixed with the current one-step results. Additional local trials
remain in their original experiment records; the tag represents the previous
published repository, not every intermediate local experiment.
