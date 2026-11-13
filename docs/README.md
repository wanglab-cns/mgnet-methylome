# Methylome Signatures for PRRT Response in GEP-NETs

**Author:** [Farnoosh Abbas Aghababazadeh](https://github.com/RibaA)  
**Contact:** [farnoosh.abbasaghababazadeh@uhn.ca](mailto:farnoosh.abbasaghababazadeh@uhn.ca)

A reproducible **whole-genome EM-seq analysis pipeline** for investigating DNA methylation signatures associated with response to peptide receptor radionuclide therapy (PRRT) in gastroenteropancreatic neuroendocrine tumors (GEP-NETs).

[![Pixi](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/prefix-dev/pixi/main/assets/badge/v0.json&style=flat-square)](https://pixi.sh/)
[![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json&style=flat-square)](https://github.com/astral-sh/ruff)
[![MkDocs Material](https://img.shields.io/badge/docs-MkDocs%20Material-gray?logo=materialformkdocs&style=flat-square)](https://squidfunk.github.io/mkdocs-material/)
![Last commit](https://img.shields.io/github/last-commit/wanglab-cns/mgnet-methylome?style=flat-square)

## Workflow

`FASTQ → fastp → bwa-meth alignment → BAM merging / duplicate marking → methylation QC → CpG calls → CpG matrix → 300-bp methylation matrix`

The pipeline uses **Slurm** for parallel processing on HPC4Health (H4H). Methylation-bias QC is reviewed before final CpG extraction. The final matrices support exploratory biomarker and PRRT-response analyses.

## Setup

This repository uses [Pixi](https://pixi.sh/latest/) for its declared environment:

```bash
git clone https://github.com/wanglab-cns/mgnet-methylome.git
cd mgnet-methylome
pixi install
```

Cluster-specific tools and reference files may require H4H modules or separately staged resources; see the repository documentation. Installing Pixi alone does not necessarily install the cluster's bioinformatics tools.

## Data and documentation

- [Data organization, metadata, QC and outputs](docs/data.md)
- [Repository documentation](docs/) — workflow details and H4H instructions

**Privacy:** Do not commit patient-level clinical metadata, controlled sequencing files, BAMs, or large analysis results to GitHub.

