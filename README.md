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
../tools/ExpansionHunter/bin/ExpansionHunter --reads ../Documents/DNA-Vicente/NG1PSZ7BE9.mm2.sortdup.bqsr.cram --reference Homo_sapiens_assembly38.fasta  --variant-catalog variant_catalog-all.json  --output-prefix VIVICO
```
```
GangSTR --bam /home/bioinfo/Documents/DNA-Vicente/NG1PSZ7BE9.mm2.sortdup.bqsr.cram --ref /home/bioinfo/Downloads/Homo_sapiens_assembly38.fasta --regions /home/bioinfo/Downloads/hg19_ver13_1.bed --out vicolinoG
```
```
./HipSTR --bams /home/bioinfo/Documents/DNA-Vicente/NG1PSZ7BE9.mm2.sortdup.bqsr.cram --fasta /home/bioinfo/Downloads/Homo_sapiens_assembly38.fasta --regions /home/bioinfo/Downloads/hg38.hipstr_reference-X.bed --str-vcf str_calls-vico.vcf.gz
```

```
bwa index Homo_sapiens_assembly38.fasta
```

