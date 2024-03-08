# BioInfo

> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.


https://storage.googleapis.com/gcp-public-data--broad-references/hg38/v0/Homo_sapiens_assembly38.fasta

https://storage.googleapis.com/gcp-public-data--broad-references/hg19/v0/Homo_sapiens_assembly19.fasta 




## Tipos de variações genéticas

![image](https://github.com/vergani/BioInfo/assets/35334365/ffa49df4-8c0c-46fa-9e6f-ec16c141e8ae)



## PIPELINE "IDEAL"?

![image](https://github.com/vergani/BioInfo/assets/35334365/828623b2-cd79-4064-89e9-3bd3fd9fb0f7)



## Setup Tools

bowtie2 - boa alternativa de alinhamento ao inves de usar bwa, mas precisa ter um indice pronto. (posso usar no galaxy qe é mais rápido).

minimap2 - até agora mais eficiente que encontrei para alinhar e mapear e cram. Testei com sucesso no galaxy

HISAT2 - ainda não testei no galaxy mas promete ser eficiente e rápido (acho que a saída é apenas BAM mesmo)

    sudo apt install samtools
    sudo apt install bwa
    sudo apt install bedtools
    sudo apt install fastp 



