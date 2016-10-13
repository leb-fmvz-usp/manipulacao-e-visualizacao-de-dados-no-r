


## Estruturas de controle

As estruturas de controle (estruturas ou expressões condicionais) são códigos cuja execução depende de uma condição. Por exemplo, se `x` e `y` são dois números a serem somados só se `x` é maior do que `y`, podemos usar uma estrutura de controle na qual a condição é `x > y`, e o código executado se a condição é verdadeira é `x + y`.  

### if

A função `if` serve justamente para criar estruturas de controle como a descrita anteriormente. A condição é definida dentro de parêntesis logo após a função `if` e o código a ser executado caso a condição seja verdadeira é agrupado por chaves.


```r
> x <- 4; y <- 3
> if (x > y) {
+   x + y
+ }
```

```
[1] 7
```

```r
> x <- 4; y <- 5
> if (x > y) {
+   x + y
+ }
```

Para continuarmos explorando estruturas de controle, criemos um banco com o número de casos por 100 mil habitantes de uma doença, por 3 anos, em 26 regiões.  


```r
> set.seed(3)
> length(letters)
```

```
[1] 26
```

```r
> banco <- data.frame(regiao = letters,
+                     ano1 = rpois(26, 50),
+                     ano2 = rpois(26, 50),
+                     ano3 = rpois(26, 50),
+                     stringsAsFactors = F)
```

Supondo que o risco da doença é alto em uma região se o total de casos é superior a 150, podemos usar a função `if` para criar uma estrutura de controle que imprime o texto `'alto'` se a condição é verdadeira para uma dada região.


```r
> if (sum(banco[banco$regiao == 'd', -1]) > 150) {
+   'alto'
+ }
```

```
[1] "alto"
```

### else

A função `else` é um complemento da função `if`. Se usarmos a estrutura de controle anterior para classificar o risco da região *m*, nenhum texto é impresso porque a condição é falsa. 


```r
> if (sum(banco[banco$regiao == 'm', -1]) > 150) {
+   'alto'
+ }
```

Com a função `else`, podemos executar um dado código (por exemplo a impressão de `baixo` se o número de casos é meni=or ou igual a 150) se a condição do `if` é falsa.


```r
> if (sum(banco[banco$regiao == 'm', -1]) > 150) {
+   'alto'
+ } else {
+   'baixo'
+ }
```

```
[1] "baixo"
```

Adicionalmente, a função `else` pode ser complementada por outra função `if`.


```r
> if (sum(banco[banco$regiao == 'm', -1]) > 145) {
+   'alto'
+ } else if (sum(banco[banco$regiao == 'm', -1]) < 135) {
+   'baixo'
+ } else {
+   'medio'
+ }
```

```
[1] "medio"
```

`ifelse` é outra alternativa para construir estruturas de controle simples. Se a condição é verdadeira, o segundo argumento é executado, caso contrário, é executado o terceiro.


```r
> ifelse(sum(banco[3, -1]) > 150, 'alta', 'baixa')
```

```
[1] "baixa"
```

### for

Os laços de repetição (loops) são estruturas de controle que como o nome sugere, repetem um código de acordo com uma dada condição. Com a função `for` que permite criar laços de repetição, um índice assume uma sequência de valores e o código da estrutura de controle é executado tantas vezes quanto valores na sequência. Por exemplo, se o índice *i* assume os valores 1, 2, e 3 (`i in 1:3`), o código da estrutura de controle é executado 3 vezes.


```r
> for (i in 1:3) {
+   print('repete')
+ }
```

```
[1] "repete"
[1] "repete"
[1] "repete"
```

Reparem que com `for` a condição é dada por `i in 1:3` (não é uma expressão lógica) e é necessário o uso da função `print` para imprimir o conteúdo de um objeto.


```r
> for (i in 1:10) {
+   print(i)
+ }
```

```
[1] 1
[1] 2
[1] 3
[1] 4
[1] 5
[1] 6
[1] 7
[1] 8
[1] 9
[1] 10
```

A ideia dos laços de repetição é automatizar procedimentos repetitivos. Por exemplo, se queremos realizar três simulações do número de cães por domicílio com base em uma distribuição de Poisson, variando o número de domicílios e o parâmetro *lambda*, para posteriormente armazenar os resultados em uma lista, duas possíveis alternativas são: realizar cada uma das simulações separadamente ou usar um laço de repetição. Criemos uma lista vazia com comprimento igual a 3 (`vector('list', length = 3)`) para armazenar os resultados e depois implementemos cada uma das alternativas.  

Alternativa 1:


```r
> caes_por_domicilio <- vector('list', length = 3)
> set.seed(2)
> caes_por_domicilio[[1]] <- rpois(20, .7)
> set.seed(2)
> caes_por_domicilio[[2]] <- rpois(30, .85)
> set.seed(2)
> caes_por_domicilio[[3]] <- rpois(50, 1)
> caes_por_domicilio
```

```
[[1]]
 [1] 0 1 1 0 2 2 0 1 0 1 1 0 1 0 0 2 3 0 0 0

[[2]]
 [1] 0 1 1 0 2 2 0 2 1 1 1 0 1 0 0 2 3 0 1 0 1 0 2 0 0 1 0 0 3 0

[[3]]
 [1] 0 1 1 0 3 3 0 2 1 1 1 0 2 0 1 2 3 0 1 0 1 1 2 0 0 1 0 0 3 0 0
[32] 0 2 2 1 1 2 0 1 0 4 0 0 0 3 2 3 0 1 2
```

Alternativa 2:


```r
> caes_por_domicilio <- vector('list', length = 3)
> domicilios <- c(20, 30, 50)
> lambdas <- c(.7, .85, 1)
> for (i in 1:length(domicilios)) {
+   set.seed(2)
+   caes_por_domicilio[[i]] <- rpois(domicilios[i], lambdas[i])
+ }
> caes_por_domicilio
```

```
[[1]]
 [1] 0 1 1 0 2 2 0 1 0 1 1 0 1 0 0 2 3 0 0 0

[[2]]
 [1] 0 1 1 0 2 2 0 2 1 1 1 0 1 0 0 2 3 0 1 0 1 0 2 0 0 1 0 0 3 0

[[3]]
 [1] 0 1 1 0 3 3 0 2 1 1 1 0 2 0 1 2 3 0 1 0 1 1 2 0 0 1 0 0 3 0 0
[32] 0 2 2 1 1 2 0 1 0 4 0 0 0 3 2 3 0 1 2
```

O laço de repetição parece mais complicado e gastamos a mesma quantidade de linhas na implementação. Porém, se queremos realizar muitas simulações, o laço de repetição é certamente mais conveniente. Por exemplo, o seguinte código realiza 100 simulações com um laço de repetição, gastando o mesmo número de linhas.


```r
> caes_por_domicilio <- vector('list', length = 100)
> domicilios <- sample(20:60, 100, r = T)
> lambdas <- runif(100, .7, 1)
> for (i in 1:length(domicilios)) {
+   set.seed(2)
+   caes_por_domicilio[[i]] <- rpois(domicilios[i], lambdas[i])
+ }
> length(caes_por_domicilio)
```

```
[1] 100
```

### while

Outra forma de criar estruturas de controle é com a função `while` que repete um dado código enquanto a condição for verdadeira. Se o código não torna falsa a condição, a execução do código continua indefinidamente. Assim, se a condição é que *i* seja menor ou igual a 5 (`i <= 5`), o objeto `i` deve ser instanciado antes de executar a estrutura de controle, e mudar dentro do código desta última até que eventualmente seja igual a 5 para que a execução seja interrompida.


```r
> i <- 1
> while(i <= 5) {
+   print(i)
+   i <- i + 1
+ }
```

```
[1] 1
[1] 2
[1] 3
[1] 4
[1] 5
```

Seguindo a mesma lógica podemos imprimir sequencialmente o nome das regiões enquanto o total de casos no segundo ano seja maior do que 40.


```r
> i <- 1
> while (banco[i, 'ano2'] > 40) {
+   print(banco[i, 'regiao'])
+   i <- i + 1
+ }
```

```
[1] "a"
[1] "b"
[1] "c"
[1] "d"
[1] "e"
[1] "f"
[1] "g"
[1] "h"
[1] "i"
[1] "j"
[1] "k"
[1] "l"
[1] "m"
[1] "n"
[1] "o"
[1] "p"
[1] "q"
```

### break

`break` é uma função que serve para interromper a execução de uma estrutura de controle. Por exemplo, podemos modificar o código anterior para imprimir sequencialmente o nome das regiões enquanto o total de casos no segundo ano seja maior do que 40 ou igual a 50.


```r
> i <- 1
> while (banco[i, 'ano2'] > 40) {
+   print(banco[i, 'regiao'])
+   i <- i + 1
+   if (banco[i, 'ano2'] == 50) {
+     break
+   }
+ }
```

```
[1] "a"
[1] "b"
[1] "c"
```

É claro que o mesmo resultado pode ser obtido criando uma condição com os dois requisitos,


```r
> i <- 1
> while (banco[i, 'ano2'] > 40 & banco[i, 'ano2'] != 50) {
+   print(banco[i, 'regiao'])
+   i <- i + 1
+ }
```

```
[1] "a"
[1] "b"
[1] "c"
```

ou usando a função `for`.


```r
> for (i in 1:nrow(banco)) {
+   if (banco[i, 'ano2'] == 50 | banco[i, 'ano2'] <= 40) {
+     break
+   }
+   print(banco[i, 'regiao'])
+ }
```

```
[1] "a"
[1] "b"
[1] "c"
```

### Estruturas de controle aninhadas

Uma estrutura de controle pode estar aninhada dentro de outra estrutura de controle, sendo possíveis vários níveis de aninhamento. O primeiro e último exemplo da seção `break`, bem como os seguintes, são exemplos de aninhamento.


```r
> if (banco[banco$regiao == 'a', 'ano1'] < 50) {
+   print('Ano 1 menor do que 50')
+   if (banco[banco$regiao == 'a', 'ano2'] < 50) {
+     'Ano 2 menor do que 50'
+   }
+ }
```

```
[1] "Ano 1 menor do que 50"
```

```
[1] "Ano 2 menor do que 50"
```


```r
> matriz <- matrix(nrow = 5, ncol = 5)
> for (i in 1:nrow(matriz)) {
+   for (j in 1:ncol(matriz)) {
+     matriz[i, j] <- i * j
+   }
+ }
> matriz
```

```
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    2    3    4    5
[2,]    2    4    6    8   10
[3,]    3    6    9   12   15
[4,]    4    8   12   16   20
[5,]    5   10   15   20   25
```

### Laços de repetição e funções vetorizadas

Existem muitas funções que evitam o uso de laços de repetição e geram resultados de forma mais eficiente. Por exemplo, para somar os elementos de posições correspondentes em dois vetores diferentes `x` e `y`, um possível laço de repetição é:


```r
> z <- c()
> x <- 1:3
> y <- 11:13
> for (i in 1:3) {
+   z[i] <- x[i] + y[i]
+ }
> z
```

```
[1] 12 14 16
```

Entretanto, é muito mais simples e eficiente usar uma função vetorizada


```r
> x + y # o operador + é uma função vetorizada.
```

```
[1] 12 14 16
```

Voltando ao `banco` que criamos no início, se queremos adicionar uma nova coluna `total` com a soma dos casos em cada região, uma estrutura de controle permite-nos acrescentar a soma do número de casos região por região.


```r
> banco$total <- 0
> for (i in 1:nrow(banco)) {
+   banco[i, 'total'] <- sum(banco[i, 2:4])
+ }
```

Porém, com a função vetorizada `rowSums` obtemos o mesmo resultado.


```r
> banco$total2 <- rowSums(banco[ , 2:4])
> all.equal(banco$total, banco$total2)
```

```
[1] TRUE
```

Para adicionar uma coluna `risco` que identifique como risco alto as regiões com mais de 150 casos e como risco baixo as regiões com 150 ou menos casos, também podemos usar um laço de repetição ou uma estratégia vetorizada com a função `which`.


```r
> banco$risco <- rep('baixo', nrow(banco))
> for (i in 1:nrow(banco)) {
+   if (sum(banco[i, 2:4]) > 150) {
+     banco[i, 'risco'] <- 'alto'
+   }
+ }
```


```r
> which(c('a', 'b', 'c') == 'b') # Lembrando a função which
```

```
[1] 2
```

```r
> banco$risco2 <- rep('baixo', nrow(banco))
> banco[which(rowSums(banco[ , 2:4]) > 150), 'risco2'] <- 'alto'
> all.equal(banco$risco, banco$risco2)
```

```
[1] TRUE
```

Até o exemplo que implementamos com dois `for` aninhados tem uma versão vetorizada.


```r
> matriz <- matrix(nrow = 5, ncol = 5)
> for (i in 1:nrow(matriz)) {
+   for (j in 1:ncol(matriz)) {
+     matriz[i, j] <- i * j
+   }
+ }
> matriz
```

```
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    2    3    4    5
[2,]    2    4    6    8   10
[3,]    3    6    9   12   15
[4,]    4    8   12   16   20
[5,]    5   10   15   20   25
```


```r
> outer(1:5, 1:5, '*')
```

```
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    2    3    4    5
[2,]    2    4    6    8   10
[3,]    3    6    9   12   15
[4,]    4    8   12   16   20
[5,]    5   10   15   20   25
```

Embora seja mais fácil e eficiente usar funções vetorizadas, há procedimentos para os que não existem versões vetorizadas e portanto, os laços de repetição são necessários.
