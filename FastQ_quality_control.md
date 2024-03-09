# Quality Assessment

## FastqC
Detalhes sobre resultado:

- [x] #739
- [ ] https://github.com/octo-org/octo-repo/issues/740
- [ ] Add delight to the experience when all tasks are complete :tada:

- Per base sequence quality - 
* Per sequence quality scores - eventualmente pode ter noise no começo, sem problema.
- Per base sequence content - é um dos indicadores mais importantes junto com o primeiro.
+ Per base N content - quando a máqunia nao tem certeza de qual letra é, coloca um N, portanto, melhor que seja zero.

`» Sequence Length Distribution`

`» Sequence Duplication Levels` - quanto maior percentual, melhor

`» Overrepresented sequences`

`» Adapter Content` - 



## trimmomatic ou trim galore
Eles descartam regiões de qualidade pobre em nossa sequencia.

### trimmomatic Pipeline:
» escolher se é single ou par
» escolher opção sobre o que ele vai fazer "cut bases off the of the read, if belo a threshold quality (trailing)"
» minimum quality requirede to keep a base: 20	
» Done!!!

### trim galore Pipeline:
» escolher se é single ou par
» Trim low-quality ends from reads in addition to adapter removal (Enter phred quality score threshold): 20
» Também é interessante gerar o relatório estatístico com informações adicionais.
» Done



