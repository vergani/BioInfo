# File Expansion (STRs)

Tools designed to target specific disease-associated repeat expansions, here are a few options:

### Sumário que algumas ferramentas:

![image](https://github.com/vergani/BioInfo/assets/35334365/8a43286d-6dac-4264-9578-6f2d746e8041)

---

## ExpansionHunter

https://github.com/Illumina/ExpansionHunter/

Is a software tool designed for detecting and genotyping repetitive DNA expansions from high-throughput sequencing data. It is specifically developed for identifying expansions associated with various genetic disorders caused by repeat expansions, such as Huntington's disease, Fragile X syndrome, myotonic dystrophy, and other repeat expansion disorders.

    ExpansionHunter --reads sample.cram --reference Homo_sapiens_assembly38.fasta  --variant-catalog variant_catalog-all.json  --output-prefix sample
    
        
    
## GangSTR 

https://github.com/gymreklab/GangSTR/

Is a tool for genome-wide profiling tandem repeats from short reads. A key advantage of GangSTR over lobSTR and HipSTR is that it can handle repeats that are longer than the read length. 

    GangSTR --bam sample.cram --ref Homo_sapiens_assembly38.fasta --regions hg19_ver13_1.bed --out sample




## HipSTR

https://hipstr-tool.github.io/HipSTR/

HipSTR is a software tool developed for detecting and genotyping STR variations from whole-genome sequencing data. It employs a hidden Markov model (HMM) to accurately identify STR expansions and contractions, including those associated with diseases. HipSTR allows for the specification of target regions and can be applied to various disease contexts where STR expansions play a role.

    ./HipSTR --bams sample.cram --fasta Homo_sapiens_assembly38.fasta --regions hg38.hipstr_reference-X.bed --str-vcf str_calls.vcf.gz





## Stripy

https://stripy.org/

A software tool developed for analyzing short tandem repeat (STR) variation in human populations. STRiPY is designed to identify and genotype STR loci from high-throughput sequencing data, with a focus on STRs in the human genome.

The main features of STRiPY include:

- Identification of STR Loci: STRiPY uses high-throughput sequencing data, such as whole-genome or whole-exome sequencing data, to identify STR loci across the human genome. It detects regions of repetitive sequences indicative of STRs.

- Genotyping STRs: Once STR loci are identified, STRiPY genotypes these loci, providing information about the repeat motif and allele lengths for each individual in the dataset.

- Population Analysis: STRiPY facilitates population-level analyses of STR variation, allowing researchers to explore patterns of STR diversity and allele frequencies across different human populations.

- Integration with Bioinformatics Pipelines: STRiPY can be integrated into bioinformatics pipelines for processing sequencing data, enabling researchers to include STR analysis as part of their genomic analyses.

STRiPY is particularly useful for studying STR variation in human populations, understanding the genetic basis of complex traits and diseases, and exploring population genetics and evolutionary studies.

### Instalando stripy:

    pip install regex
    pip install pysam
    pip install nump
    pip install numpy
    sudo apt install python3-gi python3-gi-cairo gir1.2-gtk-3.0 gir1.2-webkit2-4.1



---

## TredParse

[https://github.com/humanlongevity/tredparse/](https://github.com/humanlongevity/tredparse/)

Depois de instalado direitinho:
    $ sudo docker run -v `pwd`:`pwd` -w `pwd` humanlongevity/tredparse tred.py --tred FXS saple.cram

ou, melhor ainda:

    $ sudo docker run -v `pwd`:`pwd` -w `pwd` humanlongevity/tredparse tred.py samples.csv


onde samples é:

    #SampleKey,BAM,TRED
    t001,DRR253030-galaxy-BWA.bam,FXS

então:
    $ sudo docker run -v `pwd`:`pwd` -w `pwd` humanlongevity/tredparse tredreport.py t001.json --tsv work.tsv

    $ sudo docker run -v `pwd`:`pwd` -w `pwd` humanlongevity/tredparse tredplot.py likelihood 001.json --tred HD

