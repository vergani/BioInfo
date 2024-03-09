## VCF

The variant call format (VCF) is a generic format for storing DNA polymorphism data such as SNPs, insertions, deletions and structural variants, together with rich annotations. VCF is usually stored in a compressed manner and can be indexed for fast data retrieval of variants from a range of positions on the reference genome.

![image](https://github.com/vergani/BioInfo/assets/35334365/57be2e32-2793-46d9-a9b2-159cbc741bdb)


## FreeBayes
Variant detector
Input: CRAM or BAM
Choose parameter selection level: 	full


## bcftool mpileup
suposição: implesmente converte um BAM em um VCF file para depois usar num variant calling.
em sequida, supostamente, basta rodar o bcftools call usando como input, o output do mpileup
opções importantes:
"variants only" = YES
"output" = VCF

![image](https://github.com/vergani/BioInfo/assets/35334365/ce4f26fd-2da8-40fb-b1ee-6460e3005ae8)

mas quantas variáveis de fato tenho nesta amostra e quais os tipos de variantes encontradas?

vamos usar o bcftools counts
exemplo de saída

![image](https://github.com/vergani/BioInfo/assets/35334365/0d20debf-6805-46b4-b427-41c65dd9b05a)

importante: as vezes fica difícil distinguir uma variante verdadeira de um artefato trivial.
por isso é importante filtrar para anlisar

vfcf ilter
podemos por exemplo usar o filtro de qualidade acima de 200, para eliminar aquelas com menor certeza.

## Tipos de variações genéticas

![image](https://github.com/vergani/BioInfo/assets/35334365/ffa49df4-8c0c-46fa-9e6f-ec16c141e8ae)



