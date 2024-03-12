# Alinhar, mapear, avaliar qualidade

> [!TIP]
> Neste ponto, você já fez o controle de qualidade das amostras e trimou (se necessário).

## Index the reference genome

Nosso primeiro passo é indexar o genoma de referência para uso do BWA. 
A indexação permite que o alinhador encontre rapidamente possíveis locais de alinhamento para sequências de consulta em um genoma, o que economiza tempo durante o alinhamento. 
A indexação da referência só precisa ser executada uma vez. 
A única razão pela qual você deve criar um novo índice é se estiver trabalhando com um genoma de referência diferente ou se estiver usando uma ferramenta diferente para alinhamento.

Baixar referência HG38:
    
    wget https://storage.googleapis.com/gcp-public-data--broad-references/hg38/v0/Homo_sapiens_assembly38.fasta

Criar índice da referencia, passo necessário para usar BWA:

    bwa index Homo_sapiens_assembly38.fasta

Resultado será mais ou menos este:

    [bwa_index] Pack FASTA... 0.04 sec
    [bwa_index] Construct BWT for the packed sequence...
    [bwa_index] 1.05 seconds elapse.
    [bwa_index] Update BWT... 0.03 sec
    [bwa_index] Pack forward-only FASTA... 0.02 sec
    [bwa_index] Construct SA from BWT and Occ... 0.57 sec
    [main] Version: 0.7.17-r1188
    [main] CMD: bwa index Homo_sapiens_assembly38
    [main] Real time: 1.765 sec; CPU: 1.715 sec



## Align reads to reference genome

O processo de alinhamento consiste em escolher um genoma de referência apropriado para mapear nossas leituras e então decidir sobre um alinhador. 
Usaremos o algoritmo BWA-MEM, que é o mais recente e geralmente recomendado para consultas de alta qualidade, pois é mais rápido e preciso.
Um exemplo do bwa está abaixo. Neste caso estou usando dois arquivos fastq Paired_end usando o genoma de referência HG38:

    bwa mem Homo_sapiens_assembly38.fasta SRR19649475_1.fastq.gz SRR19649475_2.fastq.gz > SRR196494.sam




## SAM file format

The SAM file, is a tab-delimited text file that contains information for each individual read and its alignment to the genome. While we do not have time to go into detail about the features of the SAM format, the paper by Heng Li et al. provides a lot more detail on the specification.

The file begins with a header, which is optional. The header is used to describe the source of data, reference sequence, method of alignment, etc., this will change depending on the aligner being used. Following the header is the alignment section. Each line that follows corresponds to alignment information for a single read. Each alignment line has 11 mandatory fields for essential mapping information and a variable number of other fields for aligner specific information. An example entry from a SAM file is displayed below with the different fields highlighted.

» Texto plano (humam readable)
» Contém: Qualit Scores, Sequence Info (fastq) + Alignment Info + MetaData

HEADER: contém metadata (sequence dictionary, read group definitions, etc)
RECORDS:  containing structured read information (1 line per read record)

![image](https://github.com/vergani/BioInfo/assets/35334365/d04ac79e-b690-4eb2-bf3c-44e2e329b2de)


![image](https://github.com/vergani/BioInfo/assets/35334365/1d22f2ad-54a6-495c-8162-aabb442810ed)


## BAM file format

The compressed binary version of SAM is called a BAM file. We use this version to reduce size and to allow for indexing, which enables efficient random access of the data contained within the file.

We will convert the SAM file to BAM format using the samtools program with the view command and tell this command that the input is in SAM format (-S) and to output BAM format (-b):

    samtools view -S -b sample.sam > sample.bam

Next we sort the BAM file using the sort command from samtools. -o tells the command where to write the output.

    samtools sort -o sample_sorted.bam samlpe.bam 

SAM/BAM files can be sorted in multiple ways, e.g. by location of alignment on the chromosome, by read name, etc. It is important to be aware that different alignment tools will output differently sorted SAM/BAM, and different downstream tools require differently sorted alignment files as input.

You can use samtools to learn more about this bam file as well.
posso agora verificar a qualidade do alinhamento feito via samtools:

    $ samtools flagstat sample_sorted.bam

Saída será algo parecido com isso:

    28268658 + 0 in total (QC-passed reads + QC-failed reads)
    25413080 + 0 primary
    0 + 0 secondary
    2855578 + 0 supplementary
    0 + 0 duplicates
    0 + 0 primary duplicates
    28219395 + 0 mapped (99.83% : N/A)
    25363817 + 0 primary mapped (99.81% : N/A)
    25413080 + 0 paired in sequencing
    12706540 + 0 read1
    12706540 + 0 read2
    20430572 + 0 properly paired (80.39% : N/A)
    25323736 + 0 with itself and mate mapped
    40081 + 0 singletons (0.16% : N/A)
    875302 + 0 with mate mapped to a different chr
    560102 + 0 with mate mapped to a different chr (mapQ>=5)


> [!TIP]
> Ao alinhar amostras de WGS, é possível que os 23% não mapeados com sucesso, se referem a região "non-coding".
>
> Se o mapeamento ficar ruim, talves está usando um genoma de referência errado ou o fastq do passo anterior não foi bem trimado.
>
> Caso haja leituras duplicadas, podemos rodar um `MarkDuplicates`





###  A PARTIR DAQUI SÃO RASCUNHOS...
## Convertendo, alinhando, mapeando e indexando

Checar algumas info interessantes sobre o fastq:

    fastp -i DRR253030.fastq.gz



checar info interessante sobre CRAM ou BAM (se foi alinhado com HG38 ou HG19 por exemplo):

    samtools view -H sample.BAM/CRAM


Para criar índice usando bowtie2:

    bowtie2-build Homo_sapiens_assembly38.fasta Homo_sapiens_assembly38_bowtie2

Também posso baixar os índices prontos para usar no bowtie2:

    https://benlangmead.github.io/aws-indexes/bowtie

---

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


###  QualiMap BamQC 

Evaluate the quality of aligned reads data in BAM format. The tool summarizes basic statistics of the alignment (number of reads, coverage, GC-content, etc.) and produces a number of useful graphs for their interpretation.

checar esta ferramenta, nao sei se é a mesma
http://qualimap.conesalab.org/

