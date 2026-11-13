# Methylome Signatures for PRRT Response in GEP-NETs

**Author:** [Farnoosh Abbas Aghababazadeh](https://github.com/RibaA)  
**Contact:** [farnoosh.abbasaghababazadeh@uhn.ca](mailto:farnoosh.abbasaghababazadeh@uhn.ca)

A reproducible **whole-genome EM-seq analysis pipeline** for investigating DNA methylation signatures associated with response to peptide receptor radionuclide therapy (PRRT) in gastroenteropancreatic neuroendocrine tumors (GEP-NETs).

[![Pixi](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/prefix-dev/pixi/main/assets/badge/v0.json&style=flat-square)](https://pixi.sh/)
[![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json&style=flat-square)](https://github.com/astral-sh/ruff)
[![MkDocs Material](https://img.shields.io/badge/docs-MkDocs%20Material-gray?logo=materialformkdocs&style=flat-square)](https://squidfunk.github.io/mkdocs-material/)
![Last commit](https://img.shields.io/github/last-commit/wanglab-cns/mgnet-methylome?style=flat-square)

## Workflow

`FASTQ → fastp → bwa-meth → BAM merge / duplicate marking → methylation QC → CpG calls → CpG matrix → 300-bp matrix`

| Step | Analysis | Software / version | Output | Status |
| --- | --- | --- | --- | --- |
| 01–03 | FASTQ inventory, QC and trimming | **fastp** (version to confirm) | Validated, trimmed FASTQ pairs | Implemented |
| 04–05 | hg38 alignment | **bwa-meth 0.2.7**, **BWA 0.7.15**, **samtools 1.20** | Sorted and indexed BAMs per FASTQ pair | Implemented; cohort running |
| 06 | Library merge and duplicate marking | **samtools 1.20** (`merge`, `sort`, `fixmate`, `markdup`) | Duplicate-marked, indexed BAM per library; QC | Tested; additional libraries running |
| 07 | Alignment and methylation-bias QC | **samtools 1.20**, **MethylDackel 0.6.1** | QC statistics, M-bias profiles | Initial validation underway |
| 08 | CpG methylation extraction | **MethylDackel 0.6.1** | Per-library CpG methylated/unmethylated counts | Planned |
| 09 | Cross-sample CpG matrices | Python/R (versions to confirm) | CpG beta, count and coverage matrices | Planned |
| 10 | 300-bp genomic windows | Python/R (versions to confirm) | Regional beta, coverage and CpG-count matrices | Planned |

The workflow runs on **HPC4Health (H4H)** using **Slurm**. Versions above reflect the tools identified in the current H4H workflow; `fastp` and downstream analysis-runtime versions must be recorded from the installed environment. M-bias QC must be reviewed before final CpG extraction.

See [data processing and output specifications](docs/data.md) for sample/library hierarchy, QC requirements and matrix definitions.

## Setup

This repository uses [Pixi](https://pixi.sh/latest/) for its declared environment:

```bash
git clone https://github.com/wanglab-cns/mgnet-methylome.git
cd mgnet-methylome
pixi install
```

Cluster-specific tools and reference files may require H4H modules or separately staged resources; see the repository documentation. Installing Pixi alone does not necessarily install the cluster's bioinformatics tools.

## Data and documentation

- [Raw sequencing data](data/rawdata/README.md)
- [Processed data and analysis workflow](data/procdata/README.md)

