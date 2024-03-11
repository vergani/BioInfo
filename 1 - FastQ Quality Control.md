# Quality Assessment


> [!TIP]
> This lesson assumes a working understanding of the bash shell.
> This lesson also assumes some familiarity with biological concepts.
> This lesson uses algum tipo de arquivos de fastq.....



## FASTQ

FORMATO MAIS ANTIGO:

![image](https://github.com/vergani/BioInfo/assets/35334365/1de3b9d6-3493-4d7c-afd7-6c132fe74d12)

FORMATO MAIS RECENTE:

![image](https://github.com/vergani/BioInfo/assets/35334365/33028957-208a-4638-a0ea-648f1b66a94f)



## 

Prerequisites



Temos dois cabras:

bioinfo@bioinfo-VirtualBox:~/Downloads/SAMN29041673$ ls -l
-rw-rw-r-- 1 bioinfo bioinfo 655690082 mar 11 11:54 SRR19649475_1.fastq.gz
-rw-rw-r-- 1 bioinfo bioinfo 677597797 mar 11 11:01 SRR19649475_2.fastq.gz


$ fastqc *.fastq*
Started analysis of SRR19649475_1.fastq.gz
Approx 5% complete for SRR19649475_1.fastq.gz
Approx 10% complete for SRR19649475_1.fastq.gz
Approx 15% complete for SRR19649475_1.fastq.gz
Approx 20% complete for SRR19649475_1.fastq.gz
Approx 25% complete for SRR19649475_1.fastq.gz
Approx 30% complete for SRR19649475_1.fastq.gz
Approx 35% complete for SRR19649475_1.fastq.gz
Approx 40% complete for SRR19649475_1.fastq.gz
Approx 45% complete for SRR19649475_1.fastq.gz
Approx 50% complete for SRR19649475_1.fastq.gz
Approx 55% complete for SRR19649475_1.fastq.gz
Approx 60% complete for SRR19649475_1.fastq.gz
Approx 65% complete for SRR19649475_1.fastq.gz
Approx 70% complete for SRR19649475_1.fastq.gz
Approx 75% complete for SRR19649475_1.fastq.gz
Approx 80% complete for SRR19649475_1.fastq.gz
Approx 85% complete for SRR19649475_1.fastq.gz
Approx 90% complete for SRR19649475_1.fastq.gz
Approx 95% complete for SRR19649475_1.fastq.gz
Analysis complete for SRR19649475_1.fastq.gz
Started analysis of SRR19649475_2.fastq.gz
Approx 5% complete for SRR19649475_2.fastq.gz
Approx 10% complete for SRR19649475_2.fastq.gz
Approx 15% complete for SRR19649475_2.fastq.gz
Approx 20% complete for SRR19649475_2.fastq.gz
Approx 25% complete for SRR19649475_2.fastq.gz
Approx 30% complete for SRR19649475_2.fastq.gz
Approx 35% complete for SRR19649475_2.fastq.gz
Approx 40% complete for SRR19649475_2.fastq.gz
Approx 45% complete for SRR19649475_2.fastq.gz
Approx 50% complete for SRR19649475_2.fastq.gz
Approx 55% complete for SRR19649475_2.fastq.gz
Approx 60% complete for SRR19649475_2.fastq.gz
Approx 65% complete for SRR19649475_2.fastq.gz
Approx 70% complete for SRR19649475_2.fastq.gz
Approx 75% complete for SRR19649475_2.fastq.gz
Approx 80% complete for SRR19649475_2.fastq.gz
Approx 85% complete for SRR19649475_2.fastq.gz
Approx 90% complete for SRR19649475_2.fastq.gz
Approx 95% complete for SRR19649475_2.fastq.gz
Analysis complete for SRR19649475_2.fastq.gz


$ ls -l
-rw-rw-r-- 1 bioinfo bioinfo    628995 mar 11 11:58 SRR19649475_1_fastqc.html
-rw-rw-r-- 1 bioinfo bioinfo   1006582 mar 11 11:58 SRR19649475_1_fastqc.zip
-rw-rw-r-- 1 bioinfo bioinfo 655690082 mar 11 11:54 SRR19649475_1.fastq.gz
-rw-rw-r-- 1 bioinfo bioinfo    638173 mar 11 11:59 SRR19649475_2_fastqc.html
-rw-rw-r-- 1 bioinfo bioinfo   1014308 mar 11 11:59 SRR19649475_2_fastqc.zip
-rw-rw-r-- 1 bioinfo bioinfo 677597797 mar 11 11:01 SRR19649475_2.fastq.gz






## FastqC
Detalhes sobre resultado:

- Per base sequence quality - 
- Per sequence quality scores - eventualmente pode ter noise no começo, sem problema.
- Per base sequence content - é um dos indicadores mais importantes junto com o primeiro. Mostra a proporção de cada base por posição.
- Per base N content - quando a máqunia nao tem certeza de qual letra é, coloca um N, portanto, melhor que seja zero.
- Sequence Length Distribution - illumina costuma gerar cumprimentos estáveis de sequenciamento. Uma pirâmide é esperada neste gráfico.
- Sequence Duplication Levels - quanto maior percentual, melhor. Ideal que o pico do gráfico esteja sempre no lado esquerdo da imagem.
- Overrepresented sequences - geralmente zerada. Sua existência pode indicar algum tipo de contaminação na amostra.
- Adapter Content - 



## trimmomatic ou trim galore
Eles descartam regiões de qualidade pobre em nossa sequencia.

### trimmomatic Pipeline:
- escolher se é single ou par
- escolher opção sobre o que ele vai fazer "cut bases off the of the read, if belo a threshold quality (trailing)" ?????
- minimum quality requirede to keep a base: 20
- pedir relatório (-trimlog)
- Done!!!

### trim galore Pipeline:
- escolher se é single ou par
- Trim low-quality ends from reads in addition to adapter removal (Enter phred quality score threshold): 20
- Também é interessante gerar o relatório estatístico com informações adicionais.
- Done



