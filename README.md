# BioInfo
```
sudo apt install samtools
```

```
sudo apt install bwa
```

```
sudo apt install bedtools
```

```
sudo apt install fastp 
```



### Stripy
```
pip install regex
pip install pysam
pip install nump
pip install numpy
sudo apt install python3-gi python3-gi-cairo gir1.2-gtk-3.0 gir1.2-webkit2-4.1
```
--------------------

Pipeline: antes de qualquer coisa, improtante saber read lenght do fastq para só depois indexar e seguir pipeline
Também intersantes saber se é single ou paired. geralmente se tem dois arquivos R1 e R2 Paired-End Reads



https://github.com/Illumina/ExpansionHunter/blob/master/docs/02_Installation.md

```
wget https://storage.googleapis.com/gcp-public-data--broad-references/hg38/v0/Homo_sapiens_assembly38.fasta
```
```
../tools/ExpansionHunter/bin/ExpansionHunter --reads sample.cram --reference Homo_sapiens_assembly38.fasta  --variant-catalog variant_catalog-all.json  --output-prefix sample
```
```
GangSTR --bam sample.cram --ref Homo_sapiens_assembly38.fasta --regions hg19_ver13_1.bed --out sample
```
```
./HipSTR --bams sample.cram --fasta Homo_sapiens_assembly38.fasta --regions hg38.hipstr_reference-X.bed --str-vcf str_calls.vcf.gz
```


Checar algumas info interessantes sobre o fastq:
```
fastp -i DRR253030.fastq.gz
```


Quando preciso trabalhar com um fastq, antes preciso alguns ajustes e preparações:
indexar o fasta de referência:

```
bwa index Homo_sapiens_assembly38.fasta
```


Alinhar (opcional mas comendado):
```
bwa mem Homo_sapiens_assembly38.fasta sample.fastq.gz > sample.bam
```

### Galaxy workflow

https://www.ncbi.nlm.nih.gov/sra/?term=exome

Access: publici

Source: DNA

Type: exome

paired

Plataform: 'optional'

Anotar o "accession number" caso precise voltar novamente no mesmo exome.

A coluna "run" tem um ID específico que serve para importar no galaxy (ex: SRR25306983)

https://usegalaxy.org
