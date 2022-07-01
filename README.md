# VizFaDa ChIP-seq pipeline

A **Nextflow** pipeline to download [FAANG](https://data.faang.org/) ChIP-seq data,
launch the [nf-co.re/chipseq](https://nf-co.re/chipseq) in batch, and clean behind
it by removing heavy `fastq` & `bam` files, and the Nextflow `work` directory.

Used to process all FAANG ChIP-seq files for the [VizFaDa](https://viz.faang.org/) project.

## Usage
At the moment, the pipeline only runs on the [Genotoul](http://bioinfo.genotoul.fr/) compute server due to
the hard-coded launch of the nfcore/chipseq command 
[here](https://github.com/GenEpi-GenPhySE/vizfada_chipseq/blob/master/modules/chipseq_genotoul.nf#L17).

Example usage:
```bash
nextflow run lauramble/chipseq_vizfada --species 'Sus scrofa' -profile genotoul -with-report -with-trace -with-timeline -resume
```
This command will download metadata for all FAANG pig ChIP-seq, then for each distinct ChIP-seq input file, will:
- dowload .fastq files from ENA for the Input and each associated Bound samples.
- run nf-core/chipseq on these files
- remove the dowloaded `fastq` and bam `files`, as well as the temporary nextflow `/work` folder, to prevent disk saturation
- move on to the next Input sample

## Pipeline tuning

### Reference genomes
Reads will be align to Ensembl reference genomes (automatically retrieved at the begining of the pipeline) as specified by 
[this configuration file](https://github.com/GenEpi-GenPhySE/vizfada_chipseq/blob/master/conf/species.config).
