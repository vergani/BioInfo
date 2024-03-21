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

'-m' allows for multiallelic and rare-variant calling 

'-v' tells the program to output variant sites only (not every site in the genome)

'-Ov -o' indica que a saída será um VCF file


### Step 3: Filter and report the SNV variants in variant calling format (VCF)
????

    $ vcfutils.pl varFilter results/vcf/SRR2584866_variants.vcf  > results/vcf/SRR2584866_final_variants.vcf

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
