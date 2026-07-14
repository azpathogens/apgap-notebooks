# APGAP Notebooks

Template Jupyter notebooks for the Arizona Pathogen Genomics Analytics Platform (APGAP). Pre-loaded into each project's Vertex AI Workbench instance to replace Google's generic ML scaffold with a workflow tailored to pathogen genomics on GCP.

## What's in here

| Notebook | What it does |
|---|---|
| [`01-getting-started.ipynb`](notebooks/01-getting-started.ipynb) | Orientation. Confirms the kernel, lists your project's buckets, previews data, points at the other notebooks. |
| [`02-read-your-data.ipynb`](notebooks/02-read-your-data.ipynb) | Reads FASTQs from your project's analytical-dataset bucket. Per-file stats, paired-end sanity check, metadata join from `_apgap_metadata.csv`. |
| [`03-launch-a-pipeline.ipynb`](notebooks/03-launch-a-pipeline.ipynb) | Installs Nextflow, writes a `nextflow.config` for GCP Batch, runs `nf-core/viralrecon` end-to-end. Includes a samplesheet builder for your real data. |
| [`04-download-from-public-repos.ipynb`](notebooks/04-download-from-public-repos.ipynb) | Pulls sequencing data from NCBI SRA, ENA, and GenBank. Includes a sanity-check comparison between SRA and ENA copies. |
| [`05-reference.ipynb`](notebooks/05-reference.ipynb) | File-format and tool-concept reference. Skim or search; nothing to run. Covers FASTQ, FASTA, VCF, BAM/SAM, GFF, BED, samplesheet conventions, viralrecon output layout, and Nextflow / nf-core / GCP Batch primers. |
| [`06-launch-tostadas.ipynb`](notebooks/06-launch-tostadas.ipynb) | Installs Nextflow, writes a `nextflow.config` for GCP Batch, runs CDC's `tostadas` pipeline (measles VADR branch) end-to-end. Validates and annotates a consensus genome via VADR and produces an NCBI submission package. |
| [`07-launch-basespace-copy.ipynb`](notebooks/07-launch-basespace-copy.ipynb) | Copies FASTQ files from Illumina BaseSpace into your lab's ingest bucket via an APGAP Portal batch upload endpoint; the backend scrubber cascade takes over from there. Interactive widget handles the endpoint URL and service key; local transfer log prevents duplicate uploads on re-runs. |

## How to use

Open each notebook in your Vertex AI Workbench instance and run **Kernel → Restart & Run All**. Notebooks 01–04, 06, and 07 run top-to-bottom; notebook 05 is reference material.

When JupyterLab prompts "Select Kernel" on first open, pick **`Python 3 (Local)`** (under "Start python Kernel"). Don't pick `Python 3 (ipykernel) (Local)` or the PyTorch / TensorFlow ones — those are missing libraries the notebooks need and will fail at the first GCS read with `ModuleNotFoundError`.

If you see popups asking to enable Google Cloud APIs (BigQuery, Vertex AI, and friends) when the Workbench launches, you can dismiss them. These notebooks only need Cloud Storage and Cloud Batch, both of which are already enabled on your project.

Notebooks 02 and 03 auto-detect your project's analytical-dataset bucket. Override `DATASET_URI` in the parameter cell to point at a different dataset.

## Requirements

Designed for the Vertex AI Workbench managed JupyterLab environment (Python 3.10, with either `conda` or `micromamba` on the image — the install cells detect whichever is present). Notebooks install everything else they need:

- Nextflow 23.10 LTS + JDK 17 (notebooks 03 and 06)
- `sra-tools` and Biopython (notebook 04)
- Biopython (notebook 02, for FASTQ parsing)
- Illumina BaseSpace CLI 1.7.0 and `google-cloud-storage` (notebook 07)

Versions are pinned because newer Nextflow rejects `nf-core/viralrecon` 2.6.0's config. See notebook 05 for the rationale.

## Default pipeline

`nf-core/viralrecon` revision `2.6.0`. It fits paired-end short-read viral whole-genome sequencing — the shape of data in the H1N1 demo project. Swap `PIPELINE_REPO` in notebook 03 to use a different pipeline (`nf-core/rnaseq`, `nf-core/bactopia`, etc.); the launch mechanics transfer directly.

## Reporting issues

Open an [issue](https://github.com/azpathogens/apgap-notebooks/issues) with the notebook number, the cell number, and the error message.
