# Raw Data — MGNET EM-seq

This directory describes the original sequencing inputs for **Methylome Signatures for PRRT Response in GEP-NETs**. Sequencing files are stored on approved H4H storage, **not in GitHub**.

## Data overview

| Item | Description |
|---|---|
| Assay | Whole-genome enzymatic methylation sequencing (EM-seq), paired-end FASTQ |
| Sequencing provider | Ontario Institute for Cancer Research (OICR) |
| Current computational manifest | 20 cases; 22 libraries; 170 paired FASTQ sets (340 FASTQ files) |
| Biological material | Archival tumor tissue; refer to controlled sample metadata for specimen details |
| Reference genome used downstream | hg38 / GRCh38 |

The study proposal identified **49 archival tissue samples** for correlative analysis; this is distinct from the **20 cases currently represented in the sequencing manifest**. Do not assume that the numbers refer to the same dataset release.

## Manifest

The working manifest is stored on H4H at:

```text
/cluster/projects/kxin-wanggroup/procdata/MGNET/metadata/MGNET_fastq_manifest.tsv
```

| Field | Meaning |
|---|---|
| `case_id` | Study case identifier |
| `oicr_case` | Provider case identifier |
| `donor_id` | Donor identifier |
| `sample_id` | Biological specimen/sample identifier |
| `library_id` | Sequencing library identifier |
| `run` | Sequencing run or batch |
| `tissue_origin`, `tissue_type` | Specimen annotations |
| `fastq_r1`, `fastq_r2` | Full paths to mate-paired FASTQ files |

Each manifest row represents **one FASTQ pair**, not one biological sample. A library may span several runs or FASTQ pairs, and a case may have multiple libraries. Retain the `case_id` → `sample_id` → `library_id` → FASTQ-pair relationship for downstream analysis.

## Input quality control

Before alignment, confirm:

- The FASTQ pair paths exist and R1/R2 files are matched correctly.
- gzip integrity checks pass and files are readable.
- Every input pair maps to a single, correctly annotated library and sample.
- No FASTQ pair is inadvertently duplicated in the manifest.

The FASTQ processing and alignment stages are documented in [Processed Data](../procdata/README.md).

## Data governance

Original FASTQs and patient-linked or controlled-access metadata must remain in approved storage. Commit only **de-identified example manifests** to Git; do not commit raw reads, identifiable metadata, credentials, or patient-level outcomes.
