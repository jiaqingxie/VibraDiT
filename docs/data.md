# Data preparation and layout

The current models use paired arrays in **physical spectral order**, without the
old square-patch permutation. The package normalizes each source and target
independently by `(x - min(x)) / (max(x) - min(x) + 1e-8)` at access time.
Only the observed source is needed for prediction.

Place processed arrays outside the repository, or under its ignored `data/`
directory:

```text
/path/to/data/
  qm9s/          ir.npy, raman.npy
  qme14s/        ir.npy, raman.npy
  vibench_full/  ir.npy, raman.npy
  rruff/         ir.npy, raman.npy
```

Each file is a finite numeric `N × L` NumPy array. Rows must represent the same
molecular identity or measured record in both modalities. Shape equality alone
does not establish alignment. The paper's row order must be preserved when using
the published split files; these indices must not be applied to a reordered
collection.

| Collection | Bins | Coordinate convention |
|---|---:|---|
| QM9S | 3,501 | Explicit 500–4,000 cm⁻¹ grid |
| QMe14S | 3,500 | Native ordered bins; a complete physical coordinate array was not verified |
| ViBench-Full | 1,024 | Native ordered bins; documented reconstructed collection |
| RRUFF | 576 | Paired measured spectra interpolated to 500–4,000 cm⁻¹, retaining measured boundary anchors |

Dataset arrays are not included in this repository. Obtain the underlying
datasets from their original providers and preserve the preparation and row
alignment described by the manuscript. This release provides the processed-array
interface and the exact recorded split files; it does not claim a universal
raw-file download/conversion pipeline. The [dataset manifest](datasets.json)
records array checksums, lengths, split checksums and partition counts.

Seven protocols are provided: `qm9s`, `qme14s`, `vibench_full`,
`qm9s_scaffold`, `qme14s_scaffold`, `rruff` and `rruff_mineral`.
Scaffold protocols share their parent collection's arrays. RRUFF protocols
share the same paired array. Where present, calibration indices are separate
from training, validation and test and are not used for checkpoint selection.

Check dimensions, row bounds, disjoint partitions and the split checksum:

```bash
vibradit verify-data --task qm9s_vibradit_ir_seed0 --data-root /path/to/data
```

Also verify both full array checksums before reproducing a reported run:

```bash
vibradit verify-data --task qm9s_vibradit_ir_seed0 \
  --data-root /path/to/data --full-hash
```

The split names, array hashes and QC rules are part of the experiment. In
particular, the QMe14S identity split is distinct from a connectivity-exclusion
analysis. Computed collections overlap chemically; they are not independent
samples of chemical space.

## Custom data

Copy one entry in `configs/paper_tasks.json` into a separate task file. Supply
your own `data_directory`, `split_file`, spectral `length`, split checksum and
validation reference constants. Use disjoint `train`, `val` and `test` integer
arrays in an `.npz` file. Relative split paths resolve from the parent of the
configuration directory; an absolute split path is also accepted. Custom data
must not be described as reproducing the paper's scores.
