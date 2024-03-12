# 3 - Alinhar, mapear, avaliar qualidade

> [!TIP]
> Neste ponto, você já fez o controle de qualidade das amostras e trimou (se necessário).

## 3.1 Index the reference genome

Nosso primeiro passo é indexar o genoma de referência para usando a ferramenta desejada. 

### Baixar referência HG38:
    
    wget https://storage.googleapis.com/gcp-public-data--broad-references/hg38/v0/Homo_sapiens_assembly38.fasta


### Indexar arquivo de referências genômica

Algumas ferramentas necessitam indexar o arquivos de referência.
A indexação permite que o alinhador encontre rapidamente possíveis locais de alinhamento para sequências de consulta em um genoma, o que economiza tempo durante o alinhamento. 
A indexação da referência só precisa ser executada uma vez. 
A única razão pela qual você deve criar um novo índice é se estiver trabalhando com um genoma de referência diferente ou se estiver usando uma ferramenta diferente para alinhamento.

- BWA

Criar índice da referencia, passo necessário para usar BWA:

    $ bwa index Homo_sapiens_assembly38.fasta

- HISAT2

    $ hisat2-build Homo_sapiens_assembly38.fasta Homo_sapiens_assembly38_hisat2



## Align reads to reference genome

Uma vez criado o índíce, vamos alinhar nossas reads.

O processo de alinhamento consiste em escolher um genoma de referência apropriado para mapear nossas leituras e então decidir sobre um alinhador. 

- BWA

Um exemplo do bwa está abaixo. Neste caso estou usando dois arquivos fastq Paired_end usando o genoma de referência HG38, devidamente indexado:

    $ bwa mem Homo_sapiens_assembly38.fasta SRR19649475_1.fastq.gz SRR19649475_2.fastq.gz > SRR196494.sam

- HISAT2

Um exemplo do bwa está abaixo. Neste caso estou usando dois arquivos fastq Paired_end usando o genoma de referência HG38, devidamente indexado:

    $ hisat2 -x genome -1 SRR19649475_1.fastq.gz -2 SRR19649475_2.fastq.gz -S SRR19649475_hisat2.sam 2> hisat2_mapping_summary.txt



## SAM file format

O arquivo SAM é um arquivo de texto delimitado por tabulações que contém informações para cada leitura individual e seu alinhamento com o genoma. 

Basicamente é o resultado do alinhamento que executamos na seção anterior.

O arquivo começa com um cabeçalho, que é opcional. O cabeçalho é usado para descrever a fonte de dados, sequência de referência, método de alinhamento, etc., isso mudará dependendo do alinhador usado. 
Seguindo o cabeçalho está a seção de alinhamento. Cada linha a seguir corresponde às informações de alinhamento para uma única leitura. Cada linha de alinhamento possui 11 campos obrigatórios para informações essenciais de mapeamento e um número variável de outros campos para informações específicas do alinhador. 

Um exemplo de entrada de um arquivo SAM é exibido abaixo com os diferentes campos destacados.

![image](https://github.com/vergani/BioInfo/assets/35334365/d04ac79e-b690-4eb2-bf3c-44e2e329b2de)

![image](https://github.com/vergani/BioInfo/assets/35334365/1d22f2ad-54a6-495c-8162-aabb442810ed)



## BAM file format

> [!TIP]
> Uma vez superada a etapa de mapeamento das reads, vamos converter nosso SAM para BAM.

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
> Ao alinhar amostras de WGS, é possível alguns % não mapeados com sucesso, se referem a região "non-coding".
>
> Se o mapeamento ficar ruim, talves está usando um genoma de referência errado ou o fastq do passo anterior não foi bem trimado.
>
> Caso haja leituras duplicadas, podemos rodar um `MarkDuplicates`






