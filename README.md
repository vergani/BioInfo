# BioInfo

[Conceitos Básicos](conceitos_basicos.md)

[File Formats](file_formats.md)

[Setup Tools](setup.md)

[Pipelines](pipelines.md)


> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.


https://storage.googleapis.com/gcp-public-data--broad-references/hg38/v0/Homo_sapiens_assembly38.fasta

https://storage.googleapis.com/gcp-public-data--broad-references/hg19/v0/Homo_sapiens_assembly19.fasta 



---

geralmente o pipelane no que diz respeito às extensões é:
fastq -> sam -> bam -> cram


---

### Sobre as ferramentas de tandem repeats:

![image](https://github.com/vergani/BioInfo/assets/35334365/8a43286d-6dac-4264-9578-6f2d746e8041)

---

### Sequence mapping versus alignment

In summary, while sequence mapping focuses on aligning reads to a reference sequence to determine their positions, sequence alignment involves comparing two or more sequences to identify regions of similarity or homology, regardless of whether a reference sequence is involved.

---

### Tipos de variações genéticas

![image](https://github.com/vergani/BioInfo/assets/35334365/ffa49df4-8c0c-46fa-9e6f-ec16c141e8ae)


---


### Quality Assessment

#### FastqC
Detalhes sobre resultado:

- [x] #739
- [ ] https://github.com/octo-org/octo-repo/issues/740
- [ ] Add delight to the experience when all tasks are complete :tada:

`» Per base sequence quality` - 

`» Per sequence quality scores` - eventualmente pode ter noise no começo, sem problema.

`» Per base sequence content ` - é um dos indicadores mais importantes junto com o primeiro.

`» Per base N content` - quando a máqunia nao tem certeza de qual letra é, coloca um N, portanto, melhor que seja zero.

`» Sequence Length Distribution`

`» Sequence Duplication Levels` - quanto maior percentual, melhor

`» Overrepresented sequences`

`» Adapter Content` - 



#### trimmomatic ou trim galore
Eles descartam regiões de qualidade pobre em nossa sequencia.

##### trimmomatic Pipeline:
» escolher se é single ou par
» escolher opção sobre o que ele vai fazer "cut bases off the of the read, if belo a threshold quality (trailing)"
» minimum quality requirede to keep a base: 20	
» Done!!!

##### trim galore Pipeline:
» escolher se é single ou par
» Done


---





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
```
bowtie2 -p 7 -x grch38_1kgmaj/grch38_1kgmaj -1 NG1P_1.fq.gz -2 NG1P_2.fq.gz -S NG-Vi-bowtie2-hg38.sam
```

Se for um single end (usar opção -U):
```
bowtie2 -p 7 -x grch38_1kgmaj/grch38_1kgmaj -U DRR253030.fastq.gz -S DRR253030_bwt_hg38.sam
samtools view -b DRR253030_bwt_hg38.sam -o DRR_bwt_hg38_sam.bam
```
After this step, you can further compress, index, and sort the BAM file using SAMtools:
```
samtools sort alignment.bam -o alignment_sorted.bam
samtools index alignment_sorted.bam
```

```
minimap2 -t <threads> -x <preset> reference_index reads_R1.fastq reads_R2.fastq > alignment.sam
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

