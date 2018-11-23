

## Importação e exportação de arquivos

Uma forma comum de estruturar os dados é em tabelas com linhas e colunas, sendo que cada linha corresponde a uma observação, e cada coluna a uma variável medida nas observações. Por exemplo, na tabela a seguir a unidade de observação é o domicílio, há cinco observações, e duas variáveis medidas em cada observação.


| domicilio | moradores | caes\_ou\_gatos |
|---|---|---|
| 1 | 3 | sim |
| 2 | 2 | sim |
| 3 | 4 | nao |
| 4 | 2 | sim |
| 5 | 1 | nao |

Editores de planilhas como o Excel o ou Calc são uma alternativa simples e útil para criar tabelas como a anterior, quando a quantidade de dados não compromete a capacidade de processamento e armazenamento. Embora seja possível importar diretamente no R os dados armazenados em uma planilha de Excel, é mais comum salvar os dados em um arquivo CSV para depois importá-los.

Nos arquivos CSV, as tabelas são armazenadas em formato de texto, sendo que cada linha continua sendo uma observação, e as variáveis medidas em cada observação estão separadas por "," ou ";". Assim, o conteúdo da tabela anterior em um arquivo CSV separado por vírgulas seria visualizado em um editor de texto da seguinte maneira:

domicilio,moradores,caes\_ou\_gatos  
1,3,sim  
2,2,sim  
3,4,nao  
4,2,sim  
5,1,nao  

Ao criarmos uma tabela como a primeira em um editor de planilhas, podemos salvar o conteúdo em um arquivo CSV, usando a opção *salvar como*, nomeando o arquivo como *domicilios* (por exemplo), e escolhendo a extensão CSV (em Excel escolher *CSV (separado por vírgulas) (\*.csv)* no campo *Tipo*; em Calc escolher *Texto CSV (.csv)* no campo *Todos os formatos*).  

### Importação

Se no painel *Files* do RStudio escolhermos o diretório em que foi salvo o arquivo, este último aparecerá listado. Se esse diretório não for o diretório de trabalho, podemos defini-lo como tal usando a janela *More* e a opção *Set as Working Directory*. Não é necessário que o diretório com os arquivos seja o diretório de trabalho, mas isso facilita o uso de funções como a `read.csv` que importa o conteúdo de arquivos CSV.  

Ao usarmos o nome do arquivo (domicilios.csv) como argumento da função `read.csv` e designar o nome `domicilios` ao resultado da função, o conteúdo do arquivo fica armazenado em um data frame com o nome `domicilios`.


```r
> (domicilios <- read.csv('domicilios.csv'))
```

```
  domicilio moradores caes_ou_gatos
1         1         3           sim
2         2         2           sim
3         3         4           nao
4         4         2           sim
5         5         1           nao
```

Se o comando anterior gera o seguinte resultado, é porque o delimitador do arquivo CSV é ";" e não ",". Isso acontece em alguns computadores cuja configuração em português usa ";" como delimitador padrão. 


```
  domicilio.moradores.caes_ou_gatos
1                           1;3;sim
2                           2;2;sim
3                           3;4;nao
4                           4;2;sim
5                           5;1;nao
```

Nesses casos, podemos usar a função `read.csv2` cujo separador padrão é ";", ou usar a função `read.csv` e definir  o argumento `sep` como ";".


```r
> read.csv2('domicilios.csv')
> read.csv('domicilios.csv', sep = ';')
```

Tanto, `read.csv` como `read.csv2` são versões específicas para arquivos CSV, simplificadas a partir da função `read.table`. Com esta última função precisamos definir outros argumentos como para importar o arquivo (como o arquivo tem cabeçalho, `header = TRUE`).  


```r
> read.table('domicilios.csv', header = TRUE, sep = ',')
```

```
  domicilio moradores caes_ou_gatos
1         1         3           sim
2         2         2           sim
3         3         4           nao
4         4         2           sim
5         5         1           nao
```

A vantagem de `read.table` é que além de importar arquivos CSV, importa arquivos com outras extensões (ex. TXT) e outros separadores (ex. Tab).

Com qualquer das funções anteriores as colunas de caracters são transformadas em fatores por padrão.


```
 Factor w/ 2 levels "nao","sim": 2 2 1 2 1
```

Para evitar a coerção para fatores devemos usar o argumento `stringsAsFactors`.


```r
> domicilios <- read.csv('domicilios.csv', stringsAsFactors = FALSE)
> str(domicilios$caes_ou_gatos)
```

```
 chr [1:5] "sim" "sim" "nao" "sim" "nao"
```

#### Valores faltantes (missing values)

Os valores faltantes são representados no R pelo objeto `NA` e por padrão é atribuiído às células em branco das colunas numéricas. As células com o valor "NA" também são importadas como NAs. Se criarmos um arquivo CSV com o seguinte conteúdo,

| domicilio | moradores | caes\_ou\_gatos |
|---|---|---|
| 1 | 3 | sim |
| 2 |   | sim |
|   | x | nao |
| 4 | 2 | sim |
| 5 | 1 | nao |

o terceiro valor da coluna `domicilio` será um `NA`, a coluna `moradores` será importada como fator (ou caracter com `stringsAsFactors = FALSE`) devido ao "x" da terceira linha e o segundo valor dessa coluna será "".


```r
> dom3 <- read.csv('domicilios3.csv')
> str(dom3)
```

```
'data.frame':	5 obs. of  3 variables:
 $ domicilio    : int  1 2 NA 4 5
 $ moradores    : Factor w/ 5 levels "","1","2","3",..: 4 1 5 3 2
 $ caes_ou_gatos: Factor w/ 2 levels "nao","sim": 2 2 1 2 1
```

Se na planilha anterior o "x" e a célula em branco na coluna `moradores` representam valores faltantes, podemos importar o banco usando o argumento `na`.


```r
> dom3 <- read.csv('domicilios3.csv', na = c("x", ""))
> str(dom3)
```

```
'data.frame':	5 obs. of  3 variables:
 $ domicilio    : int  1 2 NA 4 5
 $ moradores    : int  3 NA NA 2 1
 $ caes_ou_gatos: Factor w/ 2 levels "nao","sim": 2 2 1 2 1
```

#### Caracteres especiais

Se criarmos um arquivo *dom* com acentos e espaços, 

| domicílio | moradores | cães ou gatos |
|---|---|---|
| 1 | 3 | sim |
| 2 | 2 | sim |
| 3 | 4 | não |
| 4 | 2 | sim |
| 5 | 1 | não |

as tentativas de importação anteriores podem gerar o seguinte erro se o sistema de codificação definido como padrão no RStudio não reconhece caracteres especiais:


```r
> read.csv('dom.csv')
```

```
Error in make.names(col.names, unique = TRUE): invalid multibyte string 1
```

Se eliminarmos o cabeçalho com o argumento `header`, o arquivo é importado, mas o cabeçalho é considerado como mais uma linha e os caracteres especiais não são adequadamente representados.  


```r
> read.csv('dom.csv', header = FALSE)
```

```
            V1        V2               V3
1 domic\xedlio moradores c\xe3es ou gatos
2            1         3              sim
3            2         2              sim
4            3         4           n\xe3o
5            4         2              sim
6            5         1           n\xe3o
```

Embora seja possível especificar a codificação *latin1* para reconhecer caracteres do português, é preferível evitar caracteres especiais nos bancos de dados.  


```r
> read.csv('dom.csv', encoding = 'latin1')
```

```
  domicílio moradores cães.ou.gatos
1         1         3           sim
2         2         2           sim
3         3         4           não
4         4         2           sim
5         5         1           não
```

#### Pacote readr

O pacote `readr` tem funções para importar arquivos que são mais rapids e consistentes. A função a ser usada depende do separador: `read_csv` para vírgula, `read_csv2`para ponto e vŕigula, `read_tsb`para Tab, `read_delim` para especificar o delimitador, entre outras. O `readr` os dados para um tibble e gera uma mensagem de aviso indicando o tipo de cada coluna.


```r
> library(readr)
> (domicilios <- read_csv('domicilios.csv'))
```

```
Parsed with column specification:
cols(
  domicilio = col_integer(),
  moradores = col_integer(),
  caes_ou_gatos = col_character()
)
```

```
# A tibble: 5 x 3
  domicilio moradores caes_ou_gatos
      <int>     <int> <chr>        
1         1         3 sim          
2         2         2 sim          
3         3         4 nao          
4         4         2 sim          
5         5         1 nao          
```


```r
> read_csv2('domicilios2.csv')
```

```
Using ',' as decimal and '.' as grouping mark. Use read_delim() for more control.
```

```
Parsed with column specification:
cols(
  domicilio = col_integer(),
  moradores = col_integer(),
  caes_ou_gatos = col_character()
)
```

```
# A tibble: 5 x 3
  domicilio moradores caes_ou_gatos
      <int>     <int> <chr>        
1         1         3 sim          
2         2         2 sim          
3         3         4 nao          
4         4         2 sim          
5         5         1 nao          
```

A especificação da codificação se faz com o argumento `encoding` da função `locale` que por sua vez é usada para definir o argumento `locale`.


```r
> read_csv('dom.csv', locale = locale(encoding = "latin1"))
```

```
Parsed with column specification:
cols(
  domicílio = col_integer(),
  moradores = col_integer(),
  `cães ou gatos` = col_character()
)
```

```
# A tibble: 5 x 3
  domicílio moradores `cães ou gatos`
      <int>     <int> <chr>          
1         1         3 sim            
2         2         2 sim            
3         3         4 não            
4         4         2 sim            
5         5         1 não            
```

Com as funções do `readr` as células em branco também são importadas como NAs, mesmo em colunas de caracteres. Se for necessário interepretar como NAs outros valores, essas funções também têm o argumento `na`.


```r
> read_csv('domicilios3.csv', na = "x")
```

```
Parsed with column specification:
cols(
  domicilio = col_integer(),
  moradores = col_integer(),
  caes_ou_gatos = col_character()
)
```

```
# A tibble: 5 x 3
  domicilio moradores caes_ou_gatos
      <int>     <int> <chr>        
1         1         3 sim          
2         2        NA sim          
3        NA        NA nao          
4         4         2 sim          
5         5         1 nao          
```


#### Interface do RStudio

O RStudio tem uma interface para importar arquivos. No painel *Envorinment*, janela *Import Daset*, as opções *From Text (base)* ou *From Text (readr)* abrem uma interface de importação que permitem definir várias opções, visualizar o arquivo de entrada e visualizar a forma em o arquivo será importado. Caso o formato de saída não seja o desejado, deverão se alterar as diferentes opções até conseguir o formato adequado. A opção *From Text (base)* primeiro abre uma interface para escolher o arquivo e uma vez feita a escolha, abre a interface de importação.

![](interface/rstudio40.png)  
<br>

A opção *From Text (readr)* abre uma única interface de importação.

![](interface/rstudio41.png)  
<br>

### Exportação

Um data frame pode ser exportado para um arquivo CSV com a função `write.csv`, especificando o nome do data frame e o nome do arquivo a ser criado. Por padrão, a função `write.csv` acrescenta uma coluna que numera as linhas, mas isso pode ser prevenido com o argumento `row.names`.

```r
> write.csv(domicilios, 'domicilios2.csv', row.names = FALSE)
```

Assim como na importação, `write.csv` e `write.csv2` são versões simplificadas de `write.table`.  

No pacote `readr` também a funções de exportação (`write_csv`, `write_csv2`, entre outras) e nẽnhuma coluna é acrescentada por padrão.


```r
> write_csv(domicilios, 'domicilios2.csv')
```

### Outras extensões

Arquivos com extensão de outros programas podem ser importados com funções dos pacotes *Hmisc*, *foreign*, *haven*, e *readxl* entre outros. Por exemplo, se o arquivo domicílios tivesse a extensão do software *STATA* ou *SPSS*, a importação seria da seguinte maneira.


```r
> library(foreign)
> domicilios <- read.dta('domicilios') # STATA
> domicilios <- read.spss('domicilios') # SPSS
```
