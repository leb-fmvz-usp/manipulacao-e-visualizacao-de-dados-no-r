

## Estilo de programação no R

Os profissionais que lidam com dados costumam investir uma parte importante dos seus esforços produzindo e usando códigos. Entretanto, a utilidade desses códigos depende da eficiência dos esforços, que por sua vez depende da aplicação de boas práticas de programação. Ainda mais importante do que a eficiência é a validade das informações geradas pelos códigos, pois conclusões obtidas de uma análise podem ser erradas, mesmo quando o código não gera mensagens de erro ou avisos. Portanto, é fundamental incorporar boas práticas de programação na rotina de trabalho, para tirar o maior proveito dos esforços realizados e diminuir a chance de produzir resultados errados.  

As boas práticas de programação incluem, entre outras coisas:  

* Construção de códigos de fácil interpretação e reprodução
* Documentação
* Automatização de tarefas repetitivas
* Controle de versionamento
* Testes de validade
* Otimização
* Revisão por pares

A consideração dos itens anteriores está além dos propósitos deste livro, mas são discutidos por outros ([Wilson e col., 2014](http://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1001745); [Wickham, 2014](http://adv-r.had.co.nz/)). No entanto, o estilo de programação, que é transversal aos itens listados, será o foco deste capítulo.  

No R, as convenções quanto ao estilo são menos uniformes em comparação com outras linguagens, e isso se evidencia na falta de um único estilo e na existência de mais de uma recomendação em relação a uma mesma questão. Os estilos propostos por [Google](https://google.github.io/styleguide/Rguide.xml) e por [Wickham, 2014](http://adv-r.had.co.nz/Style.html) são referências populares, específicas para o R. Esses estilos são a base do que veremos neste capítulo. Por outro lado, os artigos de [Wilson e col., 2014](http://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1001745) e [Wikipedia](https://en.wikipedia.org/wiki/Comment_(computer_programming)#Need_for_comments) são referências genéricas.  

### Designação

Embora o símbolo `=` funcione como operador de designação, `<-` é o padrão.


```r
> # Recomendado
> x <- 10
> 
> # Não recomendado
> x = 10
```

### Espaçamento


#### Operadores

Colocar um espaço antes e depois de operadores (`=`, `+`, `-`, `<-`, etc.).  


```r
> # Recomendado
> x <- 10
> 3 + x
> 
> 
> # Não recomendado
> x<-10
> 3+x
```

No caso do operador `:` que serve para criar sequências, não se devem colocar espaços antes ou depois.  


```r
> # Recomendado
> 1:5
> 
> # Não recomendado
> 1 : 5
```

#### Vírgulas

Colocar um espaço depois, mas não antes das vírgulas. Por exemplo, a função `seq` cria uma sequência e seus dois primeiros argumentos (`from` e `to`) definem o começo e o fim da sequência.  


```r
> # Recomendado
> seq(from = 1, to = 10)
> 
> # Não recomendado
> seq(from = 1,to = 10)
> seq(from = 1 , to = 10)
```

#### Código entre parêntesis ou colchetes

Não colocar espaço ao redor do código contido em parêntesis ou colchetes.  


```r
> x <- 1:5
> 
> # Recomendado
> mean(x)
> 
> # Não recomendado
> mean( x )
```

Como veremos no próximo capítulo (*Objetos*), os colchetes servem para selecionar (indexar) um subconjunto de elementos de um objeto. Por exemplo, o seguinte código indexa os elementos 2 a 4 do objeto `x` que é uma sequência de 1 a 5.


```r
> # Recomendado
> x[2:4]
> 
> # Nao recomendado
> x[ 2:4 ]
```

Em objetos com mais de uma dimensão como é o caso das matrizes, a seleção de elementos é feita usando colchetes e vírgulas (ver capítulo *Objetos*). No seguinte código, x é uma matriz com duas dimensões: linhas e colunas. Especificamente, uma matriz com uma sequência de 1 a 10, com cinco colunas (e consequentemente com duas linhas).  


```r
> (x <- matrix(1:10, ncol = 5))
```

```
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    3    5    7    9
[2,]    2    4    6    8   10
```

Para selecionarmos elementos da matriz, devemos especificar entre colchetes as posições das linhas e colunas a serem selecionadas. No que diz do estilo de programação, o código dentro dos colchetes não deve ter espaços ao redor, mas deve ter um espaço depois da vírgula. Assim, o seguinte código seleciona o número 8 (linha 2 e coluna 4).


```r
> # Recomendado
> x[2, 4]
```

```
[1] 8
```

```r
> # Não recomendado
> x[2,4]
```

```
[1] 8
```

```r
> x[2 , 4]
```

```
[1] 8
```

#### Parêntesis esquerdo

O parêntesis esquerdo deve ser precedido por um espaço, exceto quando segue o nome de uma função. No R existem palavras reservadas que servem para controlar o fluxo de execução dos códigos (capítulo *Estruturas de controle*) e nesses casos, o espaço precede o parêntesis esquerdo. No código a seguir a palavra reservada `if` testa a condição entre parêntesis, e se for verdadeira, executa o código contido dentro das chaves.  


```r
> # Recomendado
> if (2 > 1) {
+   'Sim'
+ }
> 
> # Não recomendado
> if(2 > 1) {
+   'Sim'
+ }
```


```r
> # Recomendado
> mean(x)
> 
> # Não recomendado
> mean (x)
```

### Chaves  

No exemplo anterior de uma estrutura de controle, o código cuja execução depende da condição, foi colocado entre chaves. A primeira chave nunca deve ficar só em uma linha e deve ser seguida por uma linha nova. A última chave deve ficar só em uma linha e deve ser seguida por uma linha nova.  


```r
> # Recomendado
> if (2 > 1) {
+   'sim'
+ }
> 
> # Não recomendado
> if (2 > 1) {'sim'}
> 
> if (2 > 1)
+ {
+   'sim'
+ }
```

Uma exceção em relação à última chave é quando as palavras reservas `if` e `else` são usadas conjuntamente. No comando a seguir o código contido nas chaves do `else` é executado se a condição testa pelo `if` é falsa. A exceção consiste na colocação do `else` na mesma linha da última chave do `if`, para evitar mensagens de erro.  


```r
> # Recomendado
> if (2 > 1) {
+   'sim'
+ } else {
+   'nao'
+ }
```

### Largura das linhas

A largura das linhas deve ser de 80 caracteres ou menos. O RStudio permite visualizar uma margem para evitar linhas com mais de 80 caracteres. Para ativar a visualização devemos entrar em *Tools* (barra de menú), posteriormente em *Global options > Code > Display*, e ativar a opção *Show margin* com *Margin column* igual a 80. Se um dado código tiver mais do que 80 carateres, o lugar certo para dividí-lo é depos de uma vírgula ou operador matemático.


```r
> # Recomendado
> paises <- c('Panamá', 'Cuba', 'Costa Rica', 'Argentina', 'Brasil',
+             'Equador', 'Colomia', 'Venezuela')
> 
> # Não recomendado
> paises <- c('Panamá', 'Cuba', 'Costa Rica', 'Argentina', 'Brasil'
+             , 'Equador', 'Colomia', 'Venezuela')
```

### Indentação

A indentação do código dentro de chaves deve conter 2 espaços; dentro de parêntesis ou colchetes deve alinhar o código de todas as linhas.


```r
> # Recomendado
> if (2 > 1) {
+   'sim'
+ }
> seq(from = 1,
+     to = 10)
> 
> # Não recomendado
> if (2 > 1) {
+ 'sim'
+ }
> seq(from = 1,
+   to = 10)
```

### Nome de objetos

O nome dos objetos deve conter só minúsculas, ser informativo, conciso, e no possível não coincidente com nomes de funções preexistentes ou palavras reservadas. Se formado por mais de uma palavra, o estilo do Google recomenda o `.` como separador e estilo do Wickham recomenda o `_`.  


```r
> ## Recomendado (não misturar os dois estilos)
> casos.dia1 <- 53 # Google
> casos_dia1 <- 53 # Wickham
> 
> 
> # Não recomendado
> CasosNoPrimeiroDia <- 53
> a <- 53
```

### Nome de arquivos

O nome dos arquivos deve ser informativo, conciso, e no possível não coincidente com nomes de funções preexistentes ou palavras reservadas. Quando formado por mais de uma palavra, o Google recomenda o `_` como separador e o Wickham o `-`.  

**Recomendado:** casos-raiva-brasil-2016.R ou casos\_raiva\_brasil\_2016.R  
**Não recomendado:** meuScript.R  

### Definição de funções

O nome das funções deve ser informativo, conciso, e no possível não coincidente com nomes de funções preexistentes ou palavras reservadas. Para nomes com mais de uma palavra, o Google recomenda usar a primeira letra de cada palavra em maiúscula, enquanto Wickham recomenda usar `_` como separador. O seguinte exemplo é apenas para ilustrar o estilo de nomeação, pois a construção de funções será abordada no no próximo capítulo.  


```r
> ## Recomendado (não misturar os dois estilos)
> 
> # Google
> MediaAritmetica <- function(x, y) {
+   (x + y) / 2
+ }
> MediaAritmetica(3, 7)
> 
> # Wickham
> media_aritmetica <- function(x, y) {
+   (x + y) / 2
+ }
> media_aritmetica(3, 7)
> 
> # Não recomendado
> foo <- function(x, y) {
+   (x + y) / 2
+ }
> foo(3, 7)
```

### Comentários

Os comentários são fundamentais para facilitar a interpretação e reproduzibilidade. Devem ser concisos e focados no esclarecimento ou contextualização, não na mecânica dos códigos. Por exemplo, se em um estudo consideram-se dados anuais no período 1990-2010 e precisa-se criar um objeto com a sequência de anos nesse período para usá-la posteriormente como variável explicativa em um modelo estátistico, o seguinte código cria a sequência.  


```r
> anos <- seq(from = 1990, to = 2010)
```

Um possível comentário para esclarecer o conteúdo do objeto `anos` é:


```r
> # Variável explicativa.
> anos <- seq(from = 1990, to = 2010)
```

O anterior contrasta com um comentário focado na mecânica


```r
> # Sequência do 2000 ao 2010. "from" define o começo e "to" o final.
> anos <- seq(from = 2000, to = 2010)
```

A documentação de funções deve esclarecer os propósitos, os argumentos e o comportamento das mesmas. A função `MediaAritmetica` que criamos anteriormente é simples e o nome é informativo, mas a título do  exemplo, sua documentação poderia ser da seguinte maneira.


```r
> MediaAritmetica <- function(x, y) {
+   # Calcula a média aritmética entre dois números.
+   # Argumentos:
+   #  x: número inteiro, real ou imaginário.
+   #  y: número inteiro, real ou imaginário.
+   # Rsultado:
+   #  Média aritmética de x e y.
+   (x + y) / 2
+ }
```

### Extensão do código

Recomenda-se que os arquivos tenham entre 50 e 200 linhas.
