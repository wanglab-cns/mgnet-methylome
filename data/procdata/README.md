# Processed Data — MGNET EM-seq

This directory documents processed sequencing and methylation data for **Methylome Signatures for PRRT Response in GEP-NETs**. Outputs are generated on H4H and are not committed to GitHub.

## Pipeline overview

**FASTQ → fastp → bwa-meth alignment → BAM merge and duplicate marking → methylation QC → CpG calls → cohort CpG matrix → 300-bp methylation matrix**

| Stage | Processing | Software / version | Output | Status (Nov 2026) |
|---|---|---|---|---|
| FASTQ preprocessing | Read QC, adapter/quality trimming | `fastp` (version to confirm) | Trimmed R1/R2 FASTQs, fastp reports | Implemented |
| Alignment | Align EM-seq reads to hg38; sort/index BAMs | `bwa-meth` 0.2.7, `bwa` 0.7.15, `samtools` 1.20 | BAM and BAI per FASTQ pair | Running across cohort |
| Library BAM preparation | Merge pairs within each library, name-sort, fixmate, coordinate-sort, mark duplicates, index | `samtools` 1.20 | Duplicate-marked, indexed BAM per library; flagstat/idxstats/markdup metrics | Validated on first library; remaining ready libraries submitted |
| Methylation QC | Review alignment QC and read-position methylation bias | `samtools` 1.20, `MethylDackel` 0.6.1 | QC metrics, M-bias tables and plots | Initial sample running |
| CpG extraction | Extract strand-merged CpG methylated and unmethylated counts after QC review | `MethylDackel` 0.6.1 | Per-library CpG table/bedGraph | Planned |
| CpG matrix | Harmonize genomic CpG positions and biological sample identifiers | Python/R (versions and implementation to confirm) | Cohort beta, methylated, unmethylated and coverage matrices | Planned |
| 300-bp aggregation | Pool CpG counts within non-overlapping hg38 windows | Python/R (versions and implementation to confirm) | 300-bp beta, coverage and contributing-CpG matrices | Planned |

*The statuses describe the pipeline at the time of documentation; check H4H completion markers and Slurm logs for live results.*

## Working directories on H4H

```text
/cluster/projects/kxin-wanggroup/procdata/MGNET/
├── metadata/              # FASTQ-pair manifest and processing lists
├── trimmed_fastq/pairs/   # fastp-trimmed reads
├── alignment/pairs/       # Individual aligned BAMs
├── alignment/libraries/   # Library-level marked-duplicate BAMs
├── bam_qc/libraries/      # Flagstat, idxstats and markdup metrics
├── methylation_qc/libraries/ # Alignment stats and M-bias plots
├── methylation_calls/     # Planned per-library CpG calls
├── matrices/              # Planned CpG and 300-bp matrices
├── scripts/               # H4H execution scripts
└── logs/                  # Slurm logs
```

The raw reads are stored separately under `/cluster/projects/kxin-wanggroup/rawdata/MGNET/`.

## Unit of analysis and sample aggregation

Processing begins **per FASTQ pair**, then merges across pairs **within the same sequencing library**. Cohort methylation matrices should use biological `sample_id` columns where appropriate, **not automatically use case IDs or library IDs**. Confirm whether multiple libraries represent the same specimen before merging their CpG counts; keep library provenance in a mapping table.

## CpG and regional methylation outputs

For a CpG with methylated count `M` and unmethylated count `U`:

```text
coverage = M + U
beta     = M / (M + U)   (when coverage > 0)
```

For a 300-bp window, calculate methylation from the **sum of counts** across contributing CpGs:

```text
regional_beta = sum(M) / (sum(M) + sum(U))
```

Retain counts and coverage, record the number of observed CpGs, and represent insufficiently covered positions as **missing**, not zero. Specify coordinate conventions and reference assembly consistently (BED/bedGraph: 0-based half-open). Standard EM-seq detects combined 5mC + 5hmC signal rather than distinguishing the two modifications.

## QC before release

- Confirm merged BAM and index integrity and review alignment/mapping metrics.
- Examine QC-failed alignments, duplicates and read-group/sample concordance.
- Review MethylDackel M-bias plots **before** selecting read-end trimming parameters for final extraction.
- Confirm CpG filtering, coverage thresholds and sample missingness before matrix construction.
- Retain per-sample counts and provenance to support downstream reanalysis.

A generated QC report or `.done` marker is **not by itself a scientific QC pass**.

## Data access

Processed BAMs, sample-level QC and methylation matrices remain in approved H4H locations. GitHub stores scripts, documentation and sanitized examples only. For input provenance, see [Raw Data](../rawdata/README.md).
