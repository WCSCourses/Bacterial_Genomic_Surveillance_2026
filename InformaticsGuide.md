# <img src="https://coursesandconferences.wellcomeconnectingscience.org/wp-content/themes/wcc_courses_and_conferences/dist/assets/svg/logo.svg" width="300" height="50"> 

# Bacterial Genomic Surveillance - Ireland and Northern Ireland Informatics Guide

**Software used during the course**      
| Software | Version (if not latest) | Module | Environment |
|-------------|--------------|----------|-------------|
| [Samtools](https://www.htslib.org/) | 1.19.2 | Mapping + Phylogeny / Genome Annotation | Conda base |
| [BWA](https://github.com/lh3/bwa) | 0.7.17 | Mapping + Phylogeny | Conda base |
| [Snippy](https://github.com/tseemann/snippy) | 4.0.2 / 0.11.0 | Mapping + Phylogeny / Genome Annotation | `conda activate snippy-env` |
| [IQ-TREE 2](https://www.iqtree.org/) | 2.4.0 | Mapping + Phylogeny | Conda base |
| [Gubbins](https://github.com/nickjcroucher/gubbins) | 3.3+ | Mapping + Phylogeny | `conda activate gubbins-env` |
| [FastBAPS](https://github.com/gtonkinhill/fastbaps) | Command-line version | Mapping + Phylogeny | Conda base |
| [FigTree](https://github.com/rambaut/figtree) | 1.4.4 | Mapping + Phylogeny | Conda base |
| [ETE Toolkit](http://etetoolkit.org/) | 3.1.3 | Mapping + Phylogeny | Conda base |
| [Unicycler](https://github.com/rrwick/Unicycler) | 0.5.1 | Assembly Comparison | Conda base |
| [Dragonflye](https://github.com/rpetit3/dragonflye) | 1.2.1 | Assembly Comparison | `conda activate dragonflye-env` |
| [QUAST](https://quast.sourceforge.net/) | 5.3.0 | Assembly Comparison | `conda activate quast-env` |
| [NUCmer](https://mummer4.github.io/) | 3.23 | Assembly Comparison | Conda base |
| [Google Chrome](https://www.google.com/chrome/) | Latest | Web Tools for GenEpi | System installation |
| [Firefox](https://www.mozilla.org/firefox/) | Latest | Web Tools for GenEpi | System installation |
| [Artemis](https://www.sanger.ac.uk/tool/artemis/) | 18.2.0 | Genome Annotation | Conda base |
| [ACT](https://www.sanger.ac.uk/tool/act/) | 18.1.x | Genome Annotation | Conda base |
| [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) | 0.12.1 | Genome Annotation | Conda base |
| [Trim Galore](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/) | 0.6.10 | Genome Annotation | `conda activate ariba-env` |
| [ARIBA](https://github.com/sanger-pathogens/ariba) | 2.14.6 | Genome Annotation | `conda activate ariba-env` |
| [SPAdes](https://github.com/ablab/spades) | 3.15.5 | Genome Annotation | Conda base |
| [ABACAS](https://abacas.sourceforge.net/) | 1.3.1 | Genome Annotation | Conda base |
| [BLAST+](https://blast.ncbi.nlm.nih.gov/) | Latest installed | Genome Annotation | Conda base |
| [Bakta](https://bakta.computational.bio/) | Latest installed | Genome Annotation | Conda base |
| [RATT](https://github.com/sanger-pathogens/RATT) | Latest installed | Comparative Genomics | Conda base |
| [Filtlong](https://github.com/rrwick/Filtlong) | 0.2.1 | Sequencing & QC | Conda base |
| [NanoPlot](https://github.com/wdecoster/NanoPlot) | 1.44.1 | Sequencing & QC | Conda base |
| [Roary](https://github.com/sanger-pathogens/Roary) | Latest installed | Pan-genome Analysis | `conda activate roary-env` |
| [Panaroo](https://github.com/gtonkinhill/panaroo) | 1.1.2 | Pan-genome Analysis | `conda activate panaroo-env` |
| [Docker](https://www.docker.com/) | 28.2.2 | Containerisation | System installation |
| [Kraken2](https://github.com/DerrickWood/kraken2) | Latest installed | Taxonomic Classification | `conda activate kraken2-env` |
| [Kraken2 Standard-8 Database](https://benlangmead.github.io/aws-indexes/k2/) | 20260226 release | Taxonomic Classification | `/home/vboxuser/databases/kraken2/standard8` |

## Conda Environments

Conda environments allow software and dependencies to be installed in isolated environments. This avoids version conflicts between tools and makes software easier to manage.

### View all available environments

```bash
conda env list
```

or

```bash
conda info --envs
```

Example output:

```text
# conda environments:
#
base                  *  /home/vboxuser/miniconda3
snippy-env               /home/vboxuser/miniconda3/envs/snippy-env
gubbins-env              /home/vboxuser/miniconda3/envs/gubbins-env
kraken2-env              /home/vboxuser/miniconda3/envs/kraken2-env
```

The `*` indicates the currently active environment.

### Activate an environment

Replace `<env-name>` with the desired environment name:

```bash
conda activate <env-name>
```

Examples:

```bash
conda activate snippy-env
conda activate gubbins-env
conda activate kraken2-env
```

### Return to the base environment

```bash
conda activate base
```

### Deactivate the current environment

```bash
conda deactivate
```

This returns you to the previous environment, typically `base`.

### Check which environment is currently active

```bash
echo $CONDA_DEFAULT_ENV
```

or

```bash
conda info --envs
```

### Check software installed in the current environment

```bash
conda list
```

### Check whether a tool is available in the current environment

```bash
which samtools
which snippy
which kraken2
```

## Informatics Set-Up
For installation and setup, please refer to the following guide:

- **[Oracle VM VirtualBox Installation Guide](https://github.com/WCSCourses/WCS_Informatics_Guides/blob/main/Installation_Guides/VM_Guide.md)** – Detailed instructions for installing and configuring VirtualBox on different operating systems. *(Note: Separate installations are needed for Intel-based and ARM-based Macs, and the VDI files will differ.)*

The Host Operating System Requirements are: <br />
- RAM requirement: 8GB (preferably 12GB) <br />
- Processor requirement: 4 processors (preferably 8) <br />
- Hard disk space: 200GB <br />
- Admin rights to the computer <br />

## Citing and Re-using Course Material

The course data are free to reuse and adapt with appropriate attribution. All course data in these repositories are licensed under the <a rel="license" href="https://creativecommons.org/licenses/by-nc-sa/4.0/">Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)</a>. <a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /> 

Each course landing page is assigned a DOI via Zenodo, providing a stable and citable reference. These DOIs can be found on the respective course landing pages and can be included in CVs or research publications, offering a professional record of the course contributions.

## Interested in attending a course?

Take a look at what courses are coming up at [Wellcome Connecting Science Courses & Conference Website](https://coursesandconferences.wellcomeconnectingscience.org/our-events/).

---

[Wellcome Connecting Science GitHub Home Page](https://github.com/WCSCourses) 

For more information or queries, feel free to contact us via the [Wellcome Connecting Science website](https://coursesandconferences.wellcomeconnectingscience.org).<br /> 
Please find us on socials [Wellcome Connecting Science Linktr](https://linktr.ee/eventswcs)

---
