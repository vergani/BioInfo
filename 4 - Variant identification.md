## Variant Calling

Independente das ferramentas usadas, o fluxo todo seria mais ou menos este:

![image](https://github.com/vergani/BioInfo/assets/35334365/e47b6e9c-8163-4d99-9e33-8b8d5603d2e2)



O formato de chamada de variante (VCF) é um formato genérico para armazenar dados de polimorfismo de DNA, como SNPs, inserções, deleções e variantes estruturais, juntamente com anotações ricas. O VCF é geralmente armazenado de forma compactada e pode ser indexado para recuperação rápida de dados de variantes de uma variedade de posições no genoma de referência.


### Step 1: Calcule a cobertura de leitura de posições no genoma

## bcftool mpileup
suposição: implesmente converte um BAM em um VCF file para depois usar num variant calling.
em sequida, supostamente, basta rodar o bcftools call usando como input, o output do mpileup
opções importantes:
`variants only = YES`
`output = VCF`

    0/0 - Homozygous to Reference
    
    0/1 - Heterozygous
    
    1/1 - Homozygous to Alternate


Pileup format provides a summarized view of the reads aligned to a reference genome at each genomic position. This information can be further used for variant calling.

The flag -O b tells bcftools to generate a bcf format output file, -o specifies where to write the output file, and -f flags the path to the reference genome:

    $ bcftools mpileup -O b -o sample_raw.bcf -f Homo_sapiens_assembly38.fasta sample.cram 

> [!TIP]
> We have now generated a raw file with coverage information for every base. arquivo.bcf é a versão "compressed" do VCF.




### Step 2: Detecte as variantes de nucleotídeo único (SNVs)

Identify SNVs using bcftools call:

    $ bcftools call -mv -Ov -o output_variants.vcf sample_raw.bcf 

`-m` allows for multiallelic and rare-variant calling 

`-v` tells the program to output variant sites only (not every site in the genome)

`-Ov -o` indica que a saída será um VCF file


### Step 3: Filtre e relate as variantes SNV em formato de chamada de variante (VCF)
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

São muitas informações, então vamos dedicar algum tempo para ter certeza de que entendemos nosso resultado.

As primeiras colunas representam as informações que temos sobre uma variação prevista.

|column 	    | info         |
| ------------- | ------------- |
|CHROM 	        |contig location where the variation occurs|
|POS 	        |position within the contig where the variation occurs|
|ID 	a .     |until we add annotation information|
|REF 	        |reference genotype (forward strand)|
|ALT 	        |sample genotype (forward strand)|
|QUAL 	        |Phred-scaled probability that the observed variant exists at this site (higher is better)|
|FILTER 	a . |if no quality filters have been applied, PASS if a filter is passed, or the name of the filters this variant failed|

Em um mundo ideal, as informações na coluna QUAL seriam tudo de que precisávamos para filtrar chamadas de variantes incorretas. No entanto, na realidade, precisamos filtrar várias outras métricas.

As duas últimas colunas contêm os genótipos e podem ser difíceis de decodificar.

|column 	    | info         |
| ------------- | ------------- |
|FORMAT 	    |lists in order the metrics presented in the final column|
|results 	    |lists the values associated with those metrics in order|

Para nosso arquivo, as métricas apresentadas são GT:PL:GQ.

|metric 	    |definition    |
| ------------- | ------------- |
|AD, DP 	|the depth per allele by sample and coverage|
|GT 	    |the genotype for the sample at this loci. For a diploid organism, the GT field indicates the two alleles carried by the sample, encoded by a 0 for the REF allele, 1 for the first ALT allele, 2 for the second ALT allele, etc. A 0/0 means homozygous reference, 0/1 is heterozygous, and 1/1 is homozygous for the alternate allele.|
|PL 	    |the likelihoods of the given genotypes|
|GQ 	    |the Phred-scaled confidence for the genotype|


Um pequeno resumo visual de todo o VCF:

![image](https://github.com/vergani/BioInfo/assets/35334365/57be2e32-2793-46d9-a9b2-159cbc741bdb)




---

## Tipos de variações genéticas

![image](https://github.com/vergani/BioInfo/assets/35334365/ffa49df4-8c0c-46fa-9e6f-ec16c141e8ae)



