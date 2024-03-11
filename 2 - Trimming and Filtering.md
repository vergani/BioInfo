# Trimming and Filtering

## FastqC
Detalhes sobre resultado:

- Per base sequence quality - 
- Per sequence quality scores - eventualmente pode ter noise no começo, sem problema.
- Per base sequence content - é um dos indicadores mais importantes junto com o primeiro. Mostra a proporção de cada base por posição.
- Per base N content - quando a máqunia nao tem certeza de qual letra é, coloca um N, portanto, melhor que seja zero.
- Sequence Length Distribution - illumina costuma gerar cumprimentos estáveis de sequenciamento. Uma pirâmide é esperada neste gráfico.
- Sequence Duplication Levels - quanto maior percentual, melhor. Ideal que o pico do gráfico esteja sempre no lado esquerdo da imagem.
- Overrepresented sequences - geralmente zerada. Sua existência pode indicar algum tipo de contaminação na amostra.
- Adapter Content - 



## trimmomatic ou trim galore
Eles descartam regiões de qualidade pobre em nossa sequencia.

### trimmomatic Pipeline:
- escolher se é single ou par
- escolher opção sobre o que ele vai fazer "cut bases off the of the read, if belo a threshold quality (trailing)" ?????
- minimum quality requirede to keep a base: 20
- pedir relatório (-trimlog)
- Done!!!

### trim galore Pipeline:
- escolher se é single ou par
- Trim low-quality ends from reads in addition to adapter removal (Enter phred quality score threshold): 20
- Também é interessante gerar o relatório estatístico com informações adicionais.
- Done
