# 3 - Alinhar, mapear, avaliar qualidade

> [!TIP]
> Neste ponto, você já fez o controle de qualidade das amostras e trimou (se necessário).

## 3.1 - Index the reference genome

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



## 3.2 - Align reads to reference genome

Uma vez criado o índíce, vamos alinhar nossas reads.

O processo de alinhamento consiste em escolher um genoma de referência apropriado para mapear nossas leituras e então decidir sobre um alinhador. 

- BWA

Um exemplo do bwa está abaixo. Neste caso estou usando dois arquivos fastq Paired_end usando o genoma de referência HG38, devidamente indexado:

    $ bwa mem Homo_sapiens_assembly38.fasta SRR19649475_1.fastq.gz SRR19649475_2.fastq.gz > SRR196494.sam

- HISAT2

Um exemplo do bwa está abaixo. Neste caso estou usando dois arquivos fastq Paired_end usando o genoma de referência HG38, devidamente indexado:

    $ hisat2 -x genome -1 SRR19649475_1.fastq.gz -2 SRR19649475_2.fastq.gz -S SRR19649475_hisat2.sam 2> hisat2_mapping_summary.txt

Ao abrir o arquivo mapping_summary.txt teremos um resumo interessante sobre nosso mapeamento:

    $ cat hisat2_mapping_summary.txt 
    12706540 reads; of these:
      12706540 (100.00%) were paired; of these:
        518434 (4.08%) aligned concordantly 0 times
        11824017 (93.05%) aligned concordantly exactly 1 time
        364089 (2.87%) aligned concordantly >1 times
        ----
        518434 pairs aligned concordantly 0 times; of these:
          64609 (12.46%) aligned discordantly 1 time
        ----
        453825 pairs aligned 0 times concordantly or discordantly; of these:
          907650 mates make up the pairs; of these:
            386984 (42.64%) aligned 0 times
            406481 (44.78%) aligned exactly 1 time
            114185 (12.58%) aligned >1 times
    98.48% overall alignment rate



Também é possível perceber que nosso arquivo SAM é muito maior que as reads originais:

    $ ls -ltr -h *.gz *.sam
    -rw-rw-r-- 1 bioinfo bioinfo 647M mar 11 11:01 SRR19649475_2.fastq.gz
    -rw-rw-r-- 1 bioinfo bioinfo 626M mar 11 11:54 SRR19649475_1.fastq.gz
    -rw-rw-r-- 1 bioinfo bioinfo 8,6G mar 12 12:36 SRR19649475_hisat2.sam

Veremos como melhorar isso nos passos que seguem.


## SAM file format

O arquivo SAM é um arquivo de texto delimitado por tabulações que contém informações para cada leitura individual e seu alinhamento com o genoma. 

Basicamente é o resultado do alinhamento que executamos na seção anterior.

O arquivo começa com um cabeçalho, que é opcional. O cabeçalho é usado para descrever a fonte de dados, sequência de referência, método de alinhamento, etc., isso mudará dependendo do alinhador usado. 
Seguindo o cabeçalho está a seção de alinhamento. Cada linha a seguir corresponde às informações de alinhamento para uma única leitura. Cada linha de alinhamento possui 11 campos obrigatórios para informações essenciais de mapeamento e um número variável de outros campos para informações específicas do alinhador. 

Um exemplo de entrada de um arquivo SAM é exibido abaixo com os diferentes campos destacados.

![image](https://github.com/vergani/BioInfo/assets/35334365/d04ac79e-b690-4eb2-bf3c-44e2e329b2de)

![image](https://github.com/vergani/BioInfo/assets/35334365/1d22f2ad-54a6-495c-8162-aabb442810ed)



## 3.3 - BAM file format

> [!TIP]
> Uma vez superada a etapa de mapeamento das reads, vamos converter nosso SAM para BAM.

A versão binária compactada do SAM é chamada de arquivo BAM. Usamos esta versão para reduzir o tamanho e permitir a indexação, o que permite acesso aleatório eficiente aos dados contidos no arquivo.

Converteremos o arquivo SAM para o formato BAM usando o programa samtools com o comando view e informaremos a este comando que a entrada está no formato SAM (-S) e a saída está no formato BAM (-b):

    $ samtools view -S -b SRR19649475_hisat2.sam > SRR19649475_hisat2.bam


### Sort BAM file by coordinates

Next we sort the BAM file using the sort command from samtools. -o tells the command where to write the output.

    $ samtools sort -o SRR19649475_hisat2_sorted.bam SRR19649475_hisat2.bam

SAM/BAM files can be sorted in multiple ways, e.g. by location of alignment on the chromosome, by read name, etc. It is important to be aware that different alignment tools will output differently sorted SAM/BAM, and different downstream tools require differently sorted alignment files as input.

You can use samtools to learn more about this bam file as well.
posso agora verificar a qualidade do alinhamento feito via samtools:

    $ samtools flagstat sample_sorted.bam

This will give you the following statistics about your sorted bam file:

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






