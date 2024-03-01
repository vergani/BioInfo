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

Quando preciso trabalhar com um fastq, antes preciso alguns ajustes e preparações:
indexar o fasta de referência:
```
bwa index Homo_sapiens_assembly38.fasta
```

alinhar (opcional mas comendado):
```
bwa mem Homo_sapiens_assembly38.fasta sample.fastq.gz > sample.bam
```
