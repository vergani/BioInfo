# File Expansion (STRs)

Tools designed to target specific disease-associated repeat expansions, here are a few options:

### Sumário que algumas ferramentas:

![image](https://github.com/vergani/BioInfo/assets/35334365/8a43286d-6dac-4264-9578-6f2d746e8041)

---

https://github.com/Illumina/ExpansionHunter/blob/master/docs/02_Installation.md

```
ExpansionHunter --reads sample.cram --reference Homo_sapiens_assembly38.fasta  --variant-catalog variant_catalog-all.json  --output-prefix sample
```
```
GangSTR --bam sample.cram --ref Homo_sapiens_assembly38.fasta --regions hg19_ver13_1.bed --out sample
```
```
./HipSTR --bams sample.cram --fasta Homo_sapiens_assembly38.fasta --regions hg38.hipstr_reference-X.bed --str-vcf str_calls.vcf.gz
```





---
### Treparse pipeline
https://github.com/humanlongevity/tredparse/

Depois de instalado direitinho:
```
$ sudo docker run -v `pwd`:`pwd` -w `pwd` humanlongevity/tredparse tred.py --tred FXS saple.cram
```
ou, melhor ainda:
```
$ sudo docker run -v `pwd`:`pwd` -w `pwd` humanlongevity/tredparse tred.py samples.csv
```

onde samples é:
```
#SampleKey,BAM,TRED
t001,DRR253030-galaxy-BWA.bam,FXS
```

então:
```
$ sudo docker run -v `pwd`:`pwd` -w `pwd` humanlongevity/tredparse tredreport.py t001.json --tsv work.tsv
```
```
$ sudo docker run -v `pwd`:`pwd` -w `pwd` humanlongevity/tredparse tredplot.py likelihood 001.json --tred HD
```
