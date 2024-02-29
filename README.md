# BioInfo
```
sudo apt install samtools
```

```
sudo apt install bwa
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

```
bwa index Homo_sapiens_assembly38.fasta
```

