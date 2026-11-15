# Sources

## Assigned paper

- Title: Long-read sequencing transcriptome quantification with lr-kallisto
- Authors: Loving et al.
- Version: bioRxiv preprint, 29 January 2025
- DOI: https://doi.org/10.1101/2024.07.19.604364
- Local file: Long-read sequencing transcriptome quantification with lr-kallisto.pdf (project root)
- Downloaded: 16 October 2026

## Course instructions

- Deliverable 1 instructions: Course submission page.
- Deadline stated on submission page: 1 November 2026, 23:59.
- Format: PDF, 1–2 pages.
- Filename: <Surname>_<Given Name>_Plan.pdf
- Submitted: 1 November 2026 via GRIPS

- Deliverable 2 instructions: "Computational Environment and Data Access" on the GRIPS course submission page.
- Deadline: 15 November 2026 at 23:59. Submit the tagged repository URL and the full commit SHA corresponding to `deliverable-2`.
- Submitted: 15 November 2026 via GRIPS

## Code sources listed in the paper

### Analysis and figure reproduction
- URL: https://github.com/pachterlab/LSRRSRLFKOTWMWMP_2024
- Access checked: 30 October 2026.
- Authors' repository commit inspected for Figure 1d–g: `97bd0aa5592304155dafc236de1c123603d33ed3`.
- [Figure 1 workflow](https://github.com/pachterlab/LSRRSRLFKOTWMWMP_2024/blob/97bd0aa5592304155dafc236de1c123603d33ed3/Figures/Figure1/run_fig1.snakefile)
- [Figure 1 environment specification](https://github.com/pachterlab/LSRRSRLFKOTWMWMP_2024/blob/97bd0aa5592304155dafc236de1c123603d33ed3/Figures/Figure1/envs/kb_python.yaml)

### kallisto
- URL: https://github.com/pachterlab/kallisto
- Use: Needed for quantifying Illumina reads (kallisto) and ONT reads
  (lr-kallisto) for Figure 1d–g.
- Access checked: 30 October 2026 (source code accessible).
- The paper reports that lr-kallisto is available in kallisto release 0.51.
- Installed on the FIDS workstation on 14 November 2026 (kallisto 0.51.1 via
  kb_python 0.29.1). Tested with synthetic data only.

## Data sources listed in the paper

### Simulation data
- URL: https://zenodo.org/records/11201284
- Use: Not required for the selected target (Figure 1d–g).
- Access checked: Not yet.
- Download status: Not downloaded for this project.

### Processed abundance matrices, reference genome and annotation files
- URL: https://zenodo.org/records/13755772
- Use: Reference genome and annotation files for Figure 1d–g, as stated
  in the Deliverable 1 plan.
- Access checked: 30 October 2026.
- Access confirmed: 14 November 2026 for the four reference files
  (`GET` request for the first two bytes of each file).
- Required files: Each of the four files below can be downloaded individually from the Zenodo record linked above.
- **GRCm39 (mm39)**
  - `mm39.fa.gz` contains the mouse reference genome sequence.
  - `mm39.gtf.gz` contains the GENCODE M32 annotation (Ensembl 109). The GTF header is dated 6 December 2022.
- **CAST/EiJ**
  - `casteij.fa.gz` contains the genome sequence for GenBank assembly `GCA_029237265.1`. Its first contig is `JARCGL010000001.1`. [NCBI BioProject](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA923323)
  - `casteij.gtf.gz` contains the annotation transferred using Liftoff v1.6.3. The GTF header does not specify the source annotation release.
- Download status: Not stored in this project.
- File sizes and checksums: To be recorded when downloaded.

## Sequencing reads for Figure 1d–g

- Source: IGVF data portal, https://data.igvf.org/
- The IGVF portal lists 8 May 2024 as the release date for all four datasets. The records do not list a separate dataset release number. The file accessions below identify the FASTQ files used in the access check.
- Access checked: 30 October 2026.
- Access confirmed: 14 November 2026 for 13 FASTQ files across the four
  datasets (`GET` request for the first two bytes of each file).
- **ONT with exome capture (13G)** — Dataset `IGVFDS4803WKTQ`
  - FASTQ: `IGVFFI2224QAWI`
- **Illumina with exome capture (13G)** — Dataset `IGVFDS9522BMQK`
  - FASTQ (next1 R1): `IGVFFI6880QBPQ`
- **ONT without exome capture (13H)** — Dataset `IGVFDS9445YYVB`
  - Ten FASTQs, one per run:
    - Runs 1–5, respectively: `IGVFFI7378FTOR`, `IGVFFI3448XIOT`, `IGVFFI1864QUQG`, `IGVFFI5928VRYG`, `IGVFFI9935UOGM`
    - Runs 6–10, respectively: `IGVFFI5938USSN`, `IGVFFI9066CRFG`, `IGVFFI7995EHHB`, `IGVFFI4230BKLF`, `IGVFFI3766OUJU`
- **Illumina without exome capture (13H)** — Dataset `IGVFDS0356VCIO`
  - FASTQ (next1 R1): `IGVFFI7638IJFU`
- Estimated size: approximately 147 GB of compressed FASTQ files.
- Download status: Not downloaded for this project.
- File sizes and checksums: To be recorded when downloaded.