# Quality Assessment


> [!TIP]
> This lesson assumes a working understanding of the bash shell.
> 
> This lesson also assumes some familiarity with biological concepts.
> 
> This lesson uses algum tipo de arquivos de fastq.....



## FASTQ

FORMATO MAIS ANTIGO:

![image](https://github.com/vergani/BioInfo/assets/35334365/1de3b9d6-3493-4d7c-afd7-6c132fe74d12)

FORMATO MAIS RECENTE:

![image](https://github.com/vergani/BioInfo/assets/35334365/33028957-208a-4638-a0ea-648f1b66a94f)



## 


Temos dois cabras:

    bioinfo@bioinfo-VirtualBox:~/Downloads/SAMN29041673$ ls -l
    -rw-rw-r-- 1 bioinfo bioinfo 655690082 mar 11 11:54 SRR19649475_1.fastq.gz
    -rw-rw-r-- 1 bioinfo bioinfo 677597797 mar 11 11:01 SRR19649475_2.fastq.gz

Rodando:

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
    
Resultado:

    $ ls -l
    -rw-rw-r-- 1 bioinfo bioinfo    628995 mar 11 11:58 SRR19649475_1_fastqc.html
    -rw-rw-r-- 1 bioinfo bioinfo   1006582 mar 11 11:58 SRR19649475_1_fastqc.zip
    -rw-rw-r-- 1 bioinfo bioinfo 655690082 mar 11 11:54 SRR19649475_1.fastq.gz
    -rw-rw-r-- 1 bioinfo bioinfo    638173 mar 11 11:59 SRR19649475_2_fastqc.html
    -rw-rw-r-- 1 bioinfo bioinfo   1014308 mar 11 11:59 SRR19649475_2_fastqc.zip
    -rw-rw-r-- 1 bioinfo bioinfo 677597797 mar 11 11:01 SRR19649475_2.fastq.gz






Outro comando intessantes para avaliar insformações de fastq:
    
    $ fastp -i SRR19649475_1.fastq.gz
    Detecting adapter sequence for read1...
    >Illumina TruSeq Adapter Read 1
    AGATCGGAAGAGCACACGTCTGAACTCCAGTCA
    
    Read1 before filtering:
    total reads: 12706540
    total bases: 1283360540
    Q20 bases: 1262248634(98.355%)
    Q30 bases: 1218638999(94.9569%)
    
    Read1 after filtering:
    total reads: 12700783
    total bases: 1281460717
    Q20 bases: 1260635731(98.3749%)
    Q30 bases: 1217156265(94.9819%)
    
    Filtering result:
    reads passed filter: 12700783
    reads failed due to low quality: 5446
    reads failed due to too many N: 0
    reads failed due to too short: 311
    reads with adapter trimmed: 87900
    bases trimmed due to adapters: 1342496
    
    Duplication rate (may be overestimated since this is SE data): 41.6776%
    
    JSON report: fastp.json
    HTML report: fastp.html
    
    fastp -i SRR19649475_1.fastq.gz 
    fastp v0.20.1, time used: 29 seconds






