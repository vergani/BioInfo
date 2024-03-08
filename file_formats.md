# File Formats

geralmente o fluxo no que diz respeito às extensões é:

fastq -> sam -> bam -> cram

---


### Sobre fastq files

FORMATO MAIS ANTIGO:

![image](https://github.com/vergani/BioInfo/assets/35334365/1de3b9d6-3493-4d7c-afd7-6c132fe74d12)

FORMATO MAIS RECENTE:

![image](https://github.com/vergani/BioInfo/assets/35334365/33028957-208a-4638-a0ea-648f1b66a94f)

---

### SAM file format (sequence alignment map)
» Texto plano (humam readable)
» Contém: Qualit Scores, Sequence Info (fastq) + Alignment Info + MetaData

HEADER: contém metadata (sequence dictionary, read group definitions, etc)
RECORDS:  containing structured read information (1 line per read record)

![image](https://github.com/vergani/BioInfo/assets/35334365/d04ac79e-b690-4eb2-bf3c-44e2e329b2de)


![image](https://github.com/vergani/BioInfo/assets/35334365/1d22f2ad-54a6-495c-8162-aabb442810ed)

---

### VCF format

The variant call format (VCF) is a generic format for storing DNA polymorphism data such as SNPs, insertions, deletions and structural variants, together with rich annotations. VCF is usually stored in a compressed manner and can be indexed for fast data retrieval of variants from a range of positions on the reference genome.

![image](https://github.com/vergani/BioInfo/assets/35334365/57be2e32-2793-46d9-a9b2-159cbc741bdb)

---

### Convertendo, alinhando, mapeando e indexando

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

