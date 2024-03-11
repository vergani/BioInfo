# Alinhar, mapear, avaliar qualidade

geralmente o fluxo no que diz respeito às extensões é:

fastq -> sam -> bam -> cram


---

## SAM file format (sequence alignment map)
» Texto plano (humam readable)
» Contém: Qualit Scores, Sequence Info (fastq) + Alignment Info + MetaData

HEADER: contém metadata (sequence dictionary, read group definitions, etc)
RECORDS:  containing structured read information (1 line per read record)

![image](https://github.com/vergani/BioInfo/assets/35334365/d04ac79e-b690-4eb2-bf3c-44e2e329b2de)


![image](https://github.com/vergani/BioInfo/assets/35334365/1d22f2ad-54a6-495c-8162-aabb442810ed)



## Convertendo, alinhando, mapeando e indexando

Checar algumas info interessantes sobre o fastq:

    fastp -i DRR253030.fastq.gz



checar info interessante sobre CRAM ou BAM (se foi alinhado com HG38 ou HG19 por exemplo):

    samtools view -H sample.BAM/CRAM


Quando preciso trabalhar com um fastq, antes preciso alguns ajustes e preparações:
indexar o fasta de referência:

    bwa index Homo_sapiens_assembly38.fasta


Para criar índice usando bowtie2:

    bowtie2-build Homo_sapiens_assembly38.fasta Homo_sapiens_assembly38_bowtie2


também posso baixar os índices prontos para usar no bownti2:

    https://benlangmead.github.io/aws-indexes/bowtie



Alinhar (opcional mas comendado):

    bwa mem Homo_sapiens_assembly38.fasta sample.fastq.gz > sample.bam


## HISAT2 (não testei isso especificamente)
A fast and sensitive alignment program
Aqui já vamos faze ro esquema de alinhar com os genes do arquivo GTF para a referencia:
  Selecionar fastq
  Print alignment summary to a file.
  Spliced alignment options (selecionar GTF do genoma de referência)

agora vamos usar o `htseq` para ver o número de leituras de um gene em particulas naquele exoma


---

Para converter dois paired end fastq para sam e depois bam:

  bowtie2 -p 7 -x grch38_1kgmaj/grch38_1kgmaj -1 NG1P_1.fq.gz -2 NG1P_2.fq.gz -S NG-Vi-bowtie2-hg38.sam


Se for um single end (usar opção -U):

    bowtie2 -p 7 -x grch38_1kgmaj/grch38_1kgmaj -U DRR253030.fastq.gz -S DRR253030_bwt_hg38.sam
    samtools view -b DRR253030_bwt_hg38.sam -o DRR_bwt_hg38_sam.bam

After this step, you can further compress, index, and sort the BAM file using SAMtools:

    samtools sort alignment.bam -o alignment_sorted.bam
    samtools index alignment_sorted.bam

    minimap2 -t <threads> -x <preset> reference_index reads_R1.fastq reads_R2.fastq > alignment.sam



## SAM, BAM, CRAM Quality Control

### samtools flagstat

Depois de rodar o mapeamento, é interessante rodar um flagstat do samtools para ver percentual de mapeamento e também se há leituras duplicadas.

Exemplo de saída do comando `samtools flagstat sample.cram`

    820180215 + 0 in total (QC-passed reads + QC-failed reads)
    818629760 + 0 primary
    0 + 0 secondary
    1550455 + 0 supplementary
    0 + 0 duplicates
    0 + 0 primary duplicates
    638756417 + 0 mapped (77.88% : N/A)
    637205962 + 0 primary mapped (77.84% : N/A)
    818629760 + 0 paired in sequencing
    409314880 + 0 read1
    409314880 + 0 read2
    623637416 + 0 properly paired (76.18% : N/A)
    634163548 + 0 with itself and mate mapped
    3042414 + 0 singletons (0.37% : N/A)
    6854466 + 0 with mate mapped to a different chr
    4351136 + 0 with mate mapped to a different chr (mapQ>=5)

Hipótese: Como estamos falando de uma amostra de WGS, é possível que os 23% não mapeados com sucesso, se referem a região "non-coding".
Se o mapeamento ficar ruim, talves está usando um genoma de referência errado ou o fastq do passo anterior não foi bem trimado.

Caso haja leituras duplicadas, podemos rodar um `MarkDuplicates`


###  QualiMap BamQC 

Evaluate the quality of aligned reads data in BAM format. The tool summarizes basic statistics of the alignment (number of reads, coverage, GC-content, etc.) and produces a number of useful graphs for their interpretation.

checar esta ferramenta, nao sei se é a mesma
http://qualimap.conesalab.org/

