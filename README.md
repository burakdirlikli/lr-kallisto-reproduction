# lr-kallisto Reproduction Project

Course: Seminar 71170 Reproducible Genomic Data Science
Semester: Winter 2026/27

## Assigned paper

Title: Long-read sequencing transcriptome quantification with lr-kallisto
Authors: Loving et al.
Version: bioRxiv preprint, 29 January 2025
DOI: https://doi.org/10.1101/2024.07.19.604364

## Project purpose

This seminar project will attempt to reproduce a selected result
from the assigned paper and document the procedure, results,
and technical difficulties.

## Current status

- Reproduction target: Figure 1d–g (agreement between ONT and Illumina
  transcript abundance estimates).
- Deliverable 1 (reproduction plan) was submitted on 1 November 2026.
- The analysis will be carried out on the FIDS workstation. Access was
  confirmed on 12 November 2026.
- The software environment was set up on the FIDS workstation and access to
  the input data was confirmed on 14 November 2026.
- Indexes from the real references have not been built yet.
- So far, I have tested the installation with synthetic data. I have not yet
  run transcript quantification from the original FASTQ reads.

## Directory structure

- Long-read sequencing transcriptome quantification with lr-kallisto.pdf:
  The assigned paper.
- assignments/: Deliverable drafts and final submissions.
- [environment.yml](environment.yml): Software dependencies for the tested analysis environment.
- notes/: Source, computational environment and AI usage records.
- reproducibility_log.md: Dated record of the project work.

## Deliverable 1: Reproduction Plan

Deadline: 1 November 2026, 23:59, as stated in the submission instructions.
Format: PDF, 1–2 pages.
Required filename: <Surname>_<Given Name>_Plan.pdf

Status: Submitted via GRIPS on 1 November 2026.

The plan describes:
1. The paper and the scientific claim to investigate.
2. The exact reproduction target and evaluation metrics.
3. Source code, datasets, computational requirements, and obstacles.

Working files: assignments/deliverable-1/draft/
Submission PDF: assignments/deliverable-1/final/

## Deliverable 2

Submitted via GRIPS on 15 November 2026. The `deliverable-2` tag points to the [submitted commit](https://github.com/burakdirlikli/lr-kallisto-reproduction/commit/cc0c0f4735578f69125aba701a74e7d30dbc0325).

### Data access

The [source list](notes/sources.md) gives the 13 FASTQ file accessions for
Figure 1d–g, grouped by dataset, and the names and versions of the four
reference files.

To obtain a FASTQ file, search for its file accession (for example,
`IGVFFI2224QAWI`) in the [IGVF data portal](https://data.igvf.org/),
open the file record and download the `.fastq.gz` file. The two reference
genomes and their annotations can be downloaded by filename from the
[authors' Zenodo record](https://doi.org/10.5281/zenodo.13755772).

On 14 November 2026, I checked access from `bs200` by requesting the first
two bytes of each of the 13 FASTQ files and four reference files. All 17
requests returned HTTP 206 and the gzip signature `1f8b`.


## Computational environment

I accessed the FIDS workstation `bs200` via the university VPN and SSH.
The [environment notes](notes/environment.md) list the operating system,
hardware, software versions and available disk space measured on 14 November 2026.

The Deliverable 1 plan assumed my own computer or Google Colab. Following
the instructor's instruction of 11 November 2026, the analysis will be
carried out on the FIDS workstation.

Local computer (documentation and project management):
- Apple M5
- 10-core CPU
- 8-core GPU
- 24 GB RAM
- 512 GB SSD capacity

## Reproduction instructions

### Install micromamba on Linux

On the `bs200` workstation (Linux x86_64), I installed micromamba in my home directory:

```bash
mkdir -p ~/.local
curl -fLsS https://micro.mamba.pm/api/micromamba/linux-64/latest | tar -xj -C ~/.local bin/micromamba
~/.local/bin/micromamba --version
```
The version reported on 14 November 2026 was `2.10.0`.

### Create and check the analysis environment

The dependencies in this project's `environment.yml` match those in the
[authors' Figure 1 environment specification](https://github.com/pachterlab/LSRRSRLFKOTWMWMP_2024/blob/97bd0aa5592304155dafc236de1c123603d33ed3/Figures/Figure1/envs/kb_python.yaml).

The YAML file specifies `kb_python` as the environment name. On `bs200`,
I used `-n lr-kallisto-fig1` to create the environment under that name.

From the project root on Linux, run:

```bash
~/.local/bin/micromamba create -n lr-kallisto-fig1 -f environment.yml -c conda-forge -y
~/.local/bin/micromamba run -n lr-kallisto-fig1 kb info
```

On `bs200`, `kb info` reported `kb_python 0.29.1`, `kallisto 0.51.1` and `bustools 0.44.1`.

### Prepare the synthetic test inputs

Run the following commands on the Linux workstation to create a 1,000-base
synthetic transcript, a transcript-to-gene mapping and one 500-base read.

```bash
mkdir -p ~/lr-kallisto-test
cd ~/lr-kallisto-test

~/.local/bin/micromamba run -n lr-kallisto-fig1 python - <<'PY'
import random
from pathlib import Path

rng = random.Random(42)
transcript = "".join(rng.choices("ACGT", k=1000))
read = transcript[:500]
quality = "I" * len(read)

Path("test.fa").write_text(f">test_tx\n{transcript}\n")
Path("test-t2g.tsv").write_text("test_tx\ttest_gene\n")
Path("test-read.fastq").write_text(f"@read1\n{read}\n+\n{quality}\n")
PY
```

### Build the synthetic test index

The authors use `k=63` for bulk ONT reads in Figure 1. On `bs200`, the default `kallisto` binary built a test index at `k=31` but rejected `k=63` because its maximum supported k-mer length was 31. I therefore used the bundled `kallisto_optoff_k64` binary to build `test-k63.idx` from the synthetic transcript in `test.fa`.

```bash
cd ~/lr-kallisto-test

~/.local/bin/micromamba run -n lr-kallisto-fig1 python - <<'PY'
from pathlib import Path
import subprocess
import kb_python

binary = Path(kb_python.__file__).parent / "bins/linux/kallisto/kallisto_optoff_k64"
subprocess.run([str(binary), "index", "-i", "test-k63.idx", "-k", "63", "test.fa"], check=True)
PY
```
### Run the synthetic long-read test

From the test directory, run `kb count` on the single synthetic read:

```bash
cd ~/lr-kallisto-test

~/.local/bin/micromamba run -n lr-kallisto-fig1 kb count \
  --long --threshold 0.8 -x bulk \
  -k 63 --opt-off --parity single \
  -i test-k63.idx -g test-t2g.tsv \
  -o test-ont-output \
  --tcc --matrix-to-directories \
  test-read.fastq
```

### Check the test output

From `~/lr-kallisto-test`, inspect the transcript name and abundance matrix:

```bash
cat test-ont-output/quant_unfiltered/transcripts.txt
cat test-ont-output/quant_unfiltered/matrix.abundance.mtx
```

The first command should print `test_tx`. The second should show:

```text
%%MatrixMarket matrix coordinate real general
1 1 1
1 1 1
```

The first numeric line describes a matrix with one row, one column and one
stored entry. The second gives the value at row 1, column 1. In this test,
the estimated abundance for the only transcript, `test_tx`, was `1`.