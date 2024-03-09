# BioInfo

Meu pipeline para WGS até o momento:

> 1 - FastQ Quality Control
> 
> Avaliar qualidade geral dos fastq obtidos via FastQC e trimar se necessário

> 2 - Alignment to referente genome
>
> Mapear e alinhar via bowtie2, minimap, bwa ou hisat2 e checar a qualidade do alinhamento

> 3 - Variant identification
>
> Single nucleotide variants (SNVs), indels, etc. Freebayes, SAMTools etc

> 4 - Annotation
>
> Comparison to public database (dbsnp, 1000 genomes; functional consequences scores

---




> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT] teste
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.


https://storage.googleapis.com/gcp-public-data--broad-references/hg38/v0/Homo_sapiens_assembly38.fasta

https://storage.googleapis.com/gcp-public-data--broad-references/hg19/v0/Homo_sapiens_assembly19.fasta 


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


## Galaxy workflow

https://www.ncbi.nlm.nih.gov/sra/?term=exome

Access: publici

Source: DNA

Type: exome

paired

Plataform: 'optional'

Anotar o "accession number" caso precise voltar novamente no mesmo exome.

A coluna "run" tem um ID específico que serve para importar no galaxy (ex: SRR25306983)


Próximos passos:
- [x] Lorem ipsum
- [ ] Lorem ipsum
- [ ] Lorem ipsum


