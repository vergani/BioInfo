## Variant Calling

Independente das ferramentas usadas, o fluxo todo seria mais ou menos este:

![image](https://github.com/vergani/BioInfo/assets/35334365/e47b6e9c-8163-4d99-9e33-8b8d5603d2e2)



The variant call format (VCF) is a generic format for storing DNA polymorphism data such as SNPs, insertions, deletions and structural variants, together with rich annotations. VCF is usually stored in a compressed manner and can be indexed for fast data retrieval of variants from a range of positions on the reference genome.

![image](https://github.com/vergani/BioInfo/assets/35334365/57be2e32-2793-46d9-a9b2-159cbc741bdb)


### Step 1: Calculate the read coverage of positions in the genome

## bcftool mpileup
suposição: implesmente converte um BAM em um VCF file para depois usar num variant calling.
em sequida, supostamente, basta rodar o bcftools call usando como input, o output do mpileup
opções importantes:
"variants only" = YES
"output" = VCF

    0/0 - Homozygous to Reference
    
    0/1 - Heterozygous
    
    1/1 - Homozygous to Alternate


Pileup format provides a summarized view of the reads aligned to a reference genome at each genomic position. This information can be further used for variant calling.

The flag -O b tells bcftools to generate a bcf format output file, -o specifies where to write the output file, and -f flags the path to the reference genome:

    $ bcftools mpileup -O b -o sample_raw.bcf -f Homo_sapiens_assembly38.fasta sample.cram 

Lembrando: arquivo.bcf é a versão "compressed" do VCF.

We have now generated a raw file with coverage information for every base.


### Step 2: Detect the single nucleotide variants (SNVs)

Identify SNVs using bcftools call:

    $ bcftools call -mv -Ov -o output_variants.vcf sample_raw.bcf 

´-m´ allows for multiallelic and rare-variant calling 

'-v' tells the program to output variant sites only (not every site in the genome)

'-Ov -o' indica que a saída será um VCF file


### Step 3: Filter and report the SNV variants in variant calling format (VCF)
????

    $ vcfutils.pl varFilter results/vcf/SRR2584866_variants.vcf  > results/vcf/SRR2584866_final_variants.vcf


Explore the VCF format:

    #CHROM  POS     ID      REF     ALT     QUAL    FILTER  INFO    FORMAT  results/bam/SRR2584866.aligned.sorted.bam
    CP000819.1      1521    .       C       T       207     .       DP=9;VDB=0.993024;SGB=-0.662043;MQSB=0.974597;MQ0F=0;AC=1;AN=1;DP4=0,0,4,5;MQ=60
    CP000819.1      1612    .       A       G       225     .       DP=13;VDB=0.52194;SGB=-0.676189;MQSB=0.950952;MQ0F=0;AC=1;AN=1;DP4=0,0,6,5;MQ=60
    CP000819.1      9092    .       A       G       225     .       DP=14;VDB=0.717543;SGB=-0.670168;MQSB=0.916482;MQ0F=0;AC=1;AN=1;DP4=0,0,7,3;MQ=60
    CP000819.1      9972    .       T       G       214     .       DP=10;VDB=0.022095;SGB=-0.670168;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,2,8;MQ=60      GT:PL
    CP000819.1      10563   .       G       A       225     .       DP=11;VDB=0.958658;SGB=-0.670168;MQSB=0.952347;MQ0F=0;AC=1;AN=1;DP4=0,0,5,5;MQ=60
    CP000819.1      22257   .       C       T       127     .       DP=5;VDB=0.0765947;SGB=-0.590765;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,2,3;MQ=60      GT:PL
    CP000819.1      38971   .       A       G       225     .       DP=14;VDB=0.872139;SGB=-0.680642;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,4,8;MQ=60      GT:PL
    CP000819.1      42306   .       A       G       225     .       DP=15;VDB=0.969686;SGB=-0.686358;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,5,9;MQ=60      GT:PL
    CP000819.1      45277   .       A       G       225     .       DP=15;VDB=0.470998;SGB=-0.680642;MQSB=0.95494;MQ0F=0;AC=1;AN=1;DP4=0,0,7,5;MQ=60
    CP000819.1      56613   .       C       G       183     .       DP=12;VDB=0.879703;SGB=-0.676189;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,8,3;MQ=60      GT:PL
    CP000819.1      62118   .       A       G       225     .       DP=19;VDB=0.414981;SGB=-0.691153;MQSB=0.906029;MQ0F=0;AC=1;AN=1;DP4=0,0,8,10;MQ=59
    CP000819.1      64042   .       G       A       225     .       DP=18;VDB=0.451328;SGB=-0.689466;MQSB=1;MQ0F=0;AC=1;AN=1;DP4=0,0,7,9;MQ=60      GT:PL

This is a lot of information, so let’s take some time to make sure we understand our output.


The first few columns represent the information we have about a predicted variation.
column 	info
CHROM 	contig location where the variation occurs
POS 	position within the contig where the variation occurs
ID 	a . until we add annotation information
REF 	reference genotype (forward strand)
ALT 	sample genotype (forward strand)
QUAL 	Phred-scaled probability that the observed variant exists at this site (higher is better)
FILTER 	a . if no quality filters have been applied, PASS if a filter is passed, or the name of the filters this variant failed


---

mas quantas variáveis de fato tenho nesta amostra e quais os tipos de variantes encontradas?

vamos usar o bcftools counts
exemplo de saída

![image](https://github.com/vergani/BioInfo/assets/35334365/0d20debf-6805-46b4-b427-41c65dd9b05a)

importante: as vezes fica difícil distinguir uma variante verdadeira de um artefato trivial.
por isso é importante filtrar para anlisar

vfcf ilter
podemos por exemplo usar o filtro de qualidade acima de 200, para eliminar aquelas com menor certeza.


---

## Tipos de variações genéticas

![image](https://github.com/vergani/BioInfo/assets/35334365/ffa49df4-8c0c-46fa-9e6f-ec16c141e8ae)






## FreeBayes
Variant detector
Input: CRAM or BAM
Choose parameter selection level: 	full
