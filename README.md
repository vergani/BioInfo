# BioInfo

https://storage.googleapis.com/gcp-public-data--broad-references/hg38/v0/Homo_sapiens_assembly38.fasta

https://storage.googleapis.com/gcp-public-data--broad-references/hg19/v0/Homo_sapiens_assembly19.fasta 

bowtie2 - boa alternativa de alinhamento ao inves de usar bwa, mas precisa ter um indice pronto. (posso usar no galaxy qe é mais rápido).

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

---

### Stripy
```
pip install regex
pip install pysam
pip install nump
pip install numpy
sudo apt install python3-gi python3-gi-cairo gir1.2-gtk-3.0 gir1.2-webkit2-4.1
```
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


Checar algumas info interessantes sobre o fastq:
```
fastp -i DRR253030.fastq.gz
```


checar info interessante sobre CRAM ou BAM (se foi alinhado com HG38 ou HG19 por exemplo):
```
samtools view -H sample.BAM/CRAM
```

Quando preciso trabalhar com um fastq, antes preciso alguns ajustes e preparações:
indexar o fasta de referência:

```
bwa index Homo_sapiens_assembly38.fasta
```

Para criar índice usando bowtie2:
```
bowtie2-build Homo_sapiens_assembly38.fasta Homo_sapiens_assembly38_bowtie2
```

também posso baixar os índices prontos para usar no bownti2:
```
https://benlangmead.github.io/aws-indexes/bowtie
```


Alinhar (opcional mas comendado):
```
bwa mem Homo_sapiens_assembly38.fasta sample.fastq.gz > sample.bam
```

---


Para converter dois paired end fastq para sam e depois bam:
Here's how you can integrate SAMtools into your Bowtie2 alignment process:
```
bowtie2 -p 7 -x grch38_1kgmaj/grch38_1kgmaj DRR253030.fastq.gz -S DRR253030_bwt_hg38.sam | samtools view -b -o DRR_bwt_hg38_sam.bam
```
After this step, you can further compress, index, and sort the BAM file using SAMtools:
```
samtools sort alignment.bam -o alignment_sorted.bam
samtools index alignment_sorted.bam
```



---
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

