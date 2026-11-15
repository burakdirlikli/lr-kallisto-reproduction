# Reproducibility Log

## 2026-10-26 — Paper reading and assignment preparation

- Started reading the lr-kallisto paper and reviewed the Deliverable 1 requirements.
- Asked Codex for explanations of technical concepts that were difficult to understand.

## 2026-10-30 — Project setup and preliminary Figure 1 feasibility check

### Project setup
- Used the existing course directory as the project root. Prepared the Deliverable 1 draft and final folders, a report template, README, .gitignore, and source and environment records.

### Preliminary check while considering the whole figure
- Inspected the authors' code and public data records. Matched the inputs for panels d–g. The exact plotting and processing steps for panels b and c could not be identified.
- Recalculated CCC, Pearson and Spearman from the published abundance tables. The values matched those shown in panels d–g at two decimal places, and these panels were considered suitable for the study.
- Asked Codex for explanations while examining and distinguishing the data, and for help with some commands.
- Estimated the compressed FASTQ inputs at approximately 147 GB. Noted possible limitations and problems.

### Next steps at the time
- Decide which panels to include in the reproduction plan, based on the available data and code.
- Initialise the Git repository.

## 2026-10-31 — Target selection and report planning

- Selected Figure 1d–g to investigate agreement between ONT and Illumina transcript abundance estimates. Panel a provided context but is not used in the report. Panels b and c were excluded from the reproduction target because their exact plotting and processing steps could not be identified.
- Planned to run quantification from FASTQ reads, match transcripts, and compare CCC, Pearson and Spearman across the capture conditions and references. Planned a small pilot to estimate runtime and memory needs.
- Reviewed the resources and decided on the report structure and figure placement.

### Next steps at the time
- Draft the English text, verify the paper citation, and prepare and check the two-page PDF.
- Initialise the Git repository.

## 2026-11-01 — Deliverable 1 report

- Completed the English plan, saved it as a PDF: assignments/deliverable-1/final/Dirlikli_Durmuş Burak_Plan.pdf and uploaded it to GRIPS.

### Next steps at the time
- Initialise the Git repository.

## 2026-11-11 — Change of computing environment

- Following the instructor's instruction that the analysis will be carried out on the FIDS workstation, running the analysis on my own computer or on Google Colab was dropped. The pilot's purpose of choosing between my own computer and Google Colab no longer applies; installation and resource testing will be handled on FIDS.

### Next steps at the time
- Initialise the Git repository.

## 2026-11-12 — Deliverable 2 workstation access

- Connected to the university VPN, logged in to the FIDS workstation via SSH and ran `hostname`. The terminal returned `bs200`.

### Next steps at the time
- Initialise the Git repository.

## 2026-11-13 — Documentation corrections

- Updated the README to reflect the current project status before starting version control.
- Corrected `notes/environment.md`: the Deliverable 1 plan assumed my own computer or Google Colab. Following the instructor's instruction of 11 November, the FIDS workstation (`bs200`) was added as the analysis environment.
- Clarified in `notes/sources.md` which of the sources listed in the paper are needed for Figure 1d–g.
- Created `notes/ai-usage.md` to record the use of AI tools in the project.

### Next steps
- Initialise the local Git repository, commit the project files and push `main` to GitHub (planned repository: https://github.com/burakdirlikli/lr-kallisto-reproduction).

## 2026-11-14 — Environment setup and data access check on FIDS

### Workstation and software environment
- Connected to `bs200` via the university VPN and SSH. Checked the CPU, memory, GPU and the available space on `/loctmp` (Debian 12). The values will be recorded in `notes/environment.md`.
- The `kallisto` command was not available on the workstation. Created the micromamba environment `lr-kallisto-fig1`. `kb info` reported `kb_python 0.29.1`, `kallisto 0.51.1` and `bustools 0.44.1`.

### Index build test: error and fix
- Built test indexes from `test.fa`, which contains only a synthetic 1,000-base transcript (`test_tx`). The `k=31` test index was built with the default `kallisto` binary.
- Building `test-k63.idx` with `k=63`, as used in the authors' bulk ONT workflow for Figure 1, failed because the default `kallisto` binary supports `k` up to 31. Deviation from the authors' workflow: they build indexes from a genome FASTA and GTF with `kb ref --opt-off` in Snakemake; for this manual installation test, `kallisto_optoff_k64` was called directly on `test.fa`.
- Ran a `kb count` test with a single synthetic long read. The abundance estimate for `test_tx` was `1`.
- Indexes from the real mm39 and CAST/EiJ references have not been built yet.

### Data access check
- The initial FASTQ HEAD request returned HTTP 403. A curl range request with a two-byte maximum stopped at the HTTP 307 redirect (curl error 63, zero bytes downloaded).
- Python GET requests for bytes 0–1 returned HTTP 206 and the gzip prefix 1f8b for all 13 selected IGVF FASTQs and four Zenodo reference files. This confirmed partial access. Whole-file checksums were not tested.
- Read the GTF headers separately to identify GENCODE M32 (Ensembl 109) for GRCm39 and Liftoff v1.6.3 as the tool used to transfer the CAST/EiJ annotation. The first CAST/EiJ FASTA record was JARCGL010000001.1. A subsequent comparison with the NCBI BioProject record linked it to assembly GCA_029237265.1.
- Asked Codex for help with some commands.

### Next steps for Deliverable 2
- Add the tested environment specification, workstation details, data identifiers and access instructions. Document how to install the software, run the synthetic test and check its output.
- Commit and push the final files, push the deliverable-2 tag and check repository access. Submit the tagged repository URL and the full commit SHA in GRIPS.

### After Deliverable 2
- Before processing the full dataset, check the available space on `bs200`. Use a private directory under `/loctmp` for temporary files and copy the results to persistent project storage after each analysis stage.
- Build indexes from the real mm39 and CAST/EiJ references.
- Measure runtime and memory on a small subset of reads before full quantification.

## 2026-11-15 — Environment specification

- Checked `~/lr-kallisto-environment.yml` on `bs200` and saved the same environment specification as `environment.yml` in the project root.
- Documented the `bs200` hardware and disk space checked on 14 November in `notes/environment.md`, noted that the paper gives no minimum hardware requirements for Figure 1d–g, and linked the environment notes from the README.
- Documented the setup and synthetic long-read test commands in the README, including how to build the k=63 test index and check the result observed on 14 November (estimated abundance of 1 for test_tx).
- Listed the 13 FASTQ accessions and four reference files in `notes/sources.md`. Added download instructions to the README and documented the successful partial access checks from `bs200` on 14 November.
- Recorded the authors' Figure 1 commit and links to the workflow and environment files in `notes/sources.md`, clarified the tested kallisto version, and updated the README directory listing to link to `environment.yml`.