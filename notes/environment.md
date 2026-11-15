# Computational Environment

## Analysis workstation (FIDS)

- Hostname: bs200
- Access: university VPN and SSH. First access confirmed on 12 November 2026.
- Operating system: Debian GNU/Linux 12 (bookworm), x86_64.
- CPU: Intel Xeon w3-2435, 8 physical cores and 16 logical CPUs.
- RAM: 62 GiB reported by free -h.
- GPU: NVIDIA RTX 4500 Ada Generation with 24,570 MiB of GPU memory.
- Temporary storage: /loctmp had a total capacity of 421 GiB, with 297 GiB available on 14 November 2026.
- The paper does not specify minimum CPU, RAM or GPU requirements for reproducing Figure 1d–g.

The Deliverable 1 plan assumed that the analysis would run on my own computer or on Google Colab. Following the instructor's instruction of 11 November 2026, the analysis will be carried out on the FIDS workstation.
Corrected on 13 November 2026.

## Local computer (documentation and project management)

### Hardware

- Chip: Apple M5
- CPU: 10 cores
- GPU: 8 cores
- RAM: 24 GB
- SSD capacity: 512 GB
- Available disk space: 352 GB.

### Operating system

- ProductVersion: 26.5.1
- BuildVersion: 25F80

### Project tools

- Git version: git version 2.50.1 (Apple Git-155)
- Editor: Visual Studio Code.

## Analysis environment

- Installed on bs200 on 14 November 2026 in the micromamba environment `lr-kallisto-fig1`.
- Micromamba 2.10.0 created the environment with Python 3.12.16.
- Versions reported by `kb info`: kb_python 0.29.1, kallisto 0.51.1, bustools 0.44.1.
- The default `kallisto` binary on `bs200` supported k-mer lengths up to 31. For the synthetic installation test, I used the bundled `kallisto_optoff_k64` binary to build the `k=63` index from `test.fa`.
- Tested only with a synthetic transcript and read. Real mm39 and CAST/EiJ indexes have not been built yet.
- Installation problems and test results are recorded in the reproducibility log.

## Verification date

14 November 2026.