

## Estruturação de bancos de dados

Um conjunto de dados pode ser estruturado de várias formas, e com frequência uma dada estrutura é mais conveniente para determinados propósitos. Nas análises estatísticas, a estrutura mais comum organiza os dados de tal forma que cada observação é uma linha e cada variável é uma coluna. Essa estrutura é conhecida como *tidy* em inglês e aqui usaremos a tradução *arrumada*. Entretanto, a coleta dos dados as vezes é mais simples usando outras estruturas e os bancos de fontes secundárias nem sempre são disponibilizados na estrutura arrumada.  

O pacote `tidyr` possui funções específicas para mudar a estrutura dos bancos de dados, e neste capítulo, seguindo a própria documentação do pacote, veremos como obter uma estrutura arrumada a partir de bancos nos quais:

* Os nomes de colunas são valores de uma variável, não variáveis em si.
* Mais de uma variável está contida em uma coluna só.

Por outro lado, as funções do pacote `broom` arrumam a estrutura dos resultados de funções, retornados em estruturas complexas (por exemplo listas aninhadas) de difícil manipulação. Veremos a funcionalidade do `broom` no contexto dos modelos de regressão, mas antes disso exploraremos o *encadeamento* de operações com `%>%`.

O pacote `tidyverse` é um pacote que carrega um conjunto de pacotes frequentemente usados na manipulação e visualização de dados. Dos pacotes vistos até o momento, o `tidyverse` carrega o `readr`, o `dplyr`, o `ggplot2` e o `tidyr`. Daqui para frente usaremos o `tidyverse` para usar o seus pacotes.

### Os nomes de colunas são valores de uma variável, não variáveis em si

Um conjunto de dados com 20 observações e 3 variáveis (setor censitário, tipo de região: rural ou urbano, e total de vacinados) pode estar estruturado da seguinte maneira:


```r
> set.seed(100)
> (caes_vacinados <- data.frame(setor = letters[1:10],
+                              rural = rpois(10, 40),
+                              urbano = rpois(10, 600)))
```

```
   setor rural urbano
1      a    36    595
2      b    29    629
3      c    45    585
4      d    40    590
5      e    42    629
6      f    36    608
7      g    42    572
8      h    37    618
9      i    40    606
10     j    40    618
```

Se os nomes das colunas `rural` e `urbano` são valores da variável *tipo de região*, a estrutura arrumada deve ter 20 linhas e 3 colunas.


```
── Attaching packages ───────────────────────────────────────── tidyverse 1.2.1 ──
```

```
✔ tibble  1.4.2     ✔ purrr   0.2.5
✔ tidyr   0.8.2     ✔ stringr 1.3.1
✔ tibble  1.4.2     ✔ forcats 0.3.0
```

```
── Conflicts ──────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
```

```
   setor   tipo vacinados
1      a  rural        36
2      b  rural        29
3      c  rural        45
4      d  rural        40
5      e  rural        42
6      f  rural        36
7      g  rural        42
8      h  rural        37
9      i  rural        40
10     j  rural        40
11     a urbano       595
12     b urbano       629
13     c urbano       585
14     d urbano       590
15     e urbano       629
16     f urbano       608
17     g urbano       572
18     h urbano       618
19     i urbano       606
20     j urbano       618
```

Com a função `gather` do `tidyr` podemos transformar a primeira estrutura na segunda especificando o banco, o nome de duas colunas *chave* e *valor*, e as colunas que não são variáveis.


```r
> library(tidyverse)
> gather(data = caes_vacinados, key = tipo, value = vacinados, rural, urbano)
```

```
   setor   tipo vacinados
1      a  rural        36
2      b  rural        29
3      c  rural        45
4      d  rural        40
5      e  rural        42
6      f  rural        36
7      g  rural        42
8      h  rural        37
9      i  rural        40
10     j  rural        40
11     a urbano       595
12     b urbano       629
13     c urbano       585
14     d urbano       590
15     e urbano       629
16     f urbano       608
17     g urbano       572
18     h urbano       618
19     i urbano       606
20     j urbano       618
```

Pelo código anterior a coluna *chave* (`key`) recebeu o nome `tipo` e armazenou os valores `rural` e `urbano`. A coluna *valor* (`value`) recebeu o nome `vacinados` e armazenou os valores correspondentes aos valores das variáveis `setor` e `tipo`.  

No lugar de especificar as colunas que não são variáveis, outra opção consiste em especificar as que são variáveis, precedidas por um sinal negativo. 


```r
> caes_vacinados <- gather(caes_vacinados, tipo, vacinados, -setor)
```

Reparem que os nomes das colunas não estão contidos em aspas. No capítulo *Objetos* vimos que para selecionar colunas de um data frame tínhamos que usar um vetor com as posições das colunas ou com os nomes entre aspas. O `tidyr` é mais flexível na hora de selecionar colunas. Vejamos alguns exemplos com o seguinte conjunto de dados que tem 15 observações e 5 variáveis (`setor`, `tipo`, `ano`, `vacinados`), e uma estrutura não arrumada.


```r
> (caes_vacinados2 <- data.frame(setor = rep(letters[1:5], 2),
+                               tipo = rep(c('rural', 'urbano'), e = 5),
+                               ano1 = rpois(5, 600),
+                               ano2 = rpois(5, 500),
+                               ano3 = rpois(5, 550)))
```

```
   setor   tipo ano1 ano2 ano3
1      a  rural  580  539  557
2      b  rural  579  496  567
3      c  rural  605  496  518
4      d  rural  571  495  582
5      e  rural  580  541  508
6      a urbano  580  539  557
7      b urbano  579  496  567
8      c urbano  605  496  518
9      d urbano  571  495  582
10     e urbano  580  541  508
```


```r
> gather(caes_vacinados2, ano, vacinados, ano1:ano3) # Colunas entre ano1 e ano3.
```


```r
> gather(caes_vacinados2, ano, vacinados, ano1, ano2, ano3)
```


```r
> gather(caes_vacinados2, ano, vacinados, -setor, -tipo)
```


```r
> gather(caes_vacinados2, ano, vacinados, -c(setor, tipo))
```


```r
> gather(caes_vacinados2, ano, vacinados, -c(setor:tipo))
```

### Mais de uma variável está contida em uma coluna só

Mais de uma variável pode estar contida em uma coluna só quando cada valor dessa coluna é a combinação dos valores de mais de uma variável, ou quando os valores dessa coluna são variáveis.  

No banco a seguir há 10 observações de 10 fêmeas identificados pela variável `id`. A coluna `idade_filhotes` contém o número de anos das fêmeas no momento do seu último parto e o número de filhotes nesse parto  (9/3: 9 anos e 3 filhotes). Assim, o banco tem 2 colunas e 3 variáveis, sendo que cada valor da coluna `idade_filhotes` contém o valor de duas variáveis: anos das fêmeas no momento do seu último parto e o número de filhotes nesse parto.


```r
> set.seed(7)
> (idades <- data.frame(id = 1:10,
+                       idade_filhotes = paste(sample(c(1:9), 10, r = T),
+                                              sample(1:12, 10, r = T),
+                                              sep = '/')))
```

```
   id idade_filhotes
1   1            9/3
2   2            4/3
3   3           2/10
4   4            1/2
5   5            3/6
6   6            8/2
7   7            4/7
8   8            9/1
9   9           2/12
10 10            5/4
```

Para arrumar a estrutura do banco podemos usar a função `separate`.


```r
> (idades <-separate(idades, idade_filhotes,
+                    into = c('idade', 'filhotes'),
+                    sep = '/'))
```

```
   id idade filhotes
1   1     9        3
2   2     4        3
3   3     2       10
4   4     1        2
5   5     3        6
6   6     8        2
7   7     4        7
8   8     9        1
9   9     2       12
10 10     5        4
```

Pelo código anterior, a coluna separada foi `idade_filhotes`, o critério de separação foi `/`, o conteúdo à esquerda de `/` passou a ser a coluna `idade` e o conteúdo à direita a coluna `filhotes`. Como a coluna inicial era tipo caractere, as resultantes também. No capítulo *Operações em bancos de dados* veremos alternativas eficientes para mudar o conteúdo dos data frames (por exemplo o tipo das colunas).


```r
> str(idades)
```

```
'data.frame':	10 obs. of  3 variables:
 $ id      : int  1 2 3 4 5 6 7 8 9 10
 $ idade   : chr  "9" "4" "2" "1" ...
 $ filhotes: chr  "3" "3" "10" "2" ...
```

Consideremos um outro banco com casos notificados e confirmados em 6 municípios diferentes.


```r
> (casos <- data.frame(municipio = letters[1:6],
+                      status = rep(c('notificados', 'confirmados'), e = 6),
+                      total = c(617, 534, 811, 233, 184, 79)))
```

```
   municipio      status total
1          a notificados   617
2          b notificados   534
3          c notificados   811
4          d notificados   233
5          e notificados   184
6          f notificados    79
7          a confirmados   617
8          b confirmados   534
9          c confirmados   811
10         d confirmados   233
11         e confirmados   184
12         f confirmados    79
```

No banco `casos` os valores da coluna `status` são variáveis. Para arrumarmos a estrutura de `casos` precisamos que cada valor diferente passe a ser uma coluna. Com a função `spread` só precisamos especificar a coluna que tem mais de uma variável, e a coluna cujos valores serão espalhados nas colunas a serem criadas.


```r
> spread(casos, status, total)
```

```
  municipio confirmados notificados
1         a         617         617
2         b         534         534
3         c         811         811
4         d         233         233
5         e         184         184
6         f          79          79
```

### Encadeamento

`spread` é a função inversa de `gather`.


```r
> gather(spread(casos, status, total), status, total, -municipio)
```

```
   municipio      status total
1          a confirmados   617
2          b confirmados   534
3          c confirmados   811
4          d confirmados   233
5          e confirmados   184
6          f confirmados    79
7          a notificados   617
8          b notificados   534
9          c notificados   811
10         d notificados   233
11         e notificados   184
12         f notificados    79
```

No código anterior aplicamos a função `spread` ao banco `casos` (`spread(casos, status, total)`) e o resultado foi o primeiro argumento da função `gather`.  

No lugar de escrevermos um código que é executado de dentro para fora, podemos usar o operador `%>%` que aplica o resultado do que está à esquerda como primeiro argumento do que está à direita.


```r
> exp(sqrt(log(10)))
```

```
[1] 4.560477
```


```r
> 10 %>% log() %>% sqrt() %>% exp()
```

```
[1] 4.560477
```


```r
> casos %>%
+   spread(status, total) %>%
+   gather(status, total, -municipio)
```

```
   municipio      status total
1          a confirmados   617
2          b confirmados   534
3          c confirmados   811
4          d confirmados   233
5          e confirmados   184
6          f confirmados    79
7          a notificados   617
8          b notificados   534
9          c notificados   811
10         d notificados   233
11         e notificados   184
12         f notificados    79
```

### Data frame do resultado de uma função

Muitas funções retornam resultados em estruturas complexas (por exemplo listas aninhadas) de difícil manipulação. Um exemplo é a função `lm` que ajusta uma regressão linear. A teoria da regressão linear está além do escopo deste livro, mas para os exemplos a seguir, podemos pensar que dado um banco com uma variável desfecho *y* e uma variável explicativa *x*, uma regressão linear simples determina a associação linear entre *x* e *y*, e quantifica entre outras coisas a proporção da variação em *y* explicada por *x* (R^2 ajustado), e a significância estatística da associação (valor *p* para o coeficiente estimado de *x*).  

Assim, dado o seguinte banco


```r
> set.seed(4)
> x <- rnorm(500, 50, 10)
> banco <- data.frame(grupo = rep(1:10, e = 50),
+                     y = 1 + 0.4 * x + rnorm(500, 0, 10),
+                     x = x)
> head(banco)
```

```
  grupo         y        x
1     1  5.420216 52.16755
2     1 10.630271 44.57507
3     1  7.782182 58.91145
4     1 27.625572 55.95981
5     1 24.054606 66.35618
6     1 23.233784 56.89275
```

podemos ajustar uma regressão linear e ver o sumário do resultado.


```r
> modelo <- lm(y ~ x, data = banco)
> summary(modelo)
```

```

Call:
lm(formula = y ~ x, data = banco)

Residuals:
     Min       1Q   Median       3Q      Max 
-25.7194  -5.9298   0.0961   6.8075  26.7512 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) -0.77163    2.27262   -0.34    0.734    
x            0.42765    0.04488    9.53   <2e-16 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 9.713 on 498 degrees of freedom
Multiple R-squared:  0.1542,	Adjusted R-squared:  0.1525 
F-statistic: 90.81 on 1 and 498 DF,  p-value: < 2.2e-16
```

Na função `lm` o primeiro argumento é uma fórmula (`variável resposta ~ varfiável explicativa`) que usa variáveis contidas no banco especificado no argumento `data`.  

A estrutura de `summary(modelo)` é uma lista aninhada 


```r
> str(summary(modelo))
```

```
List of 11
 $ call         : language lm(formula = y ~ x, data = banco)
 $ terms        :Classes 'terms', 'formula'  language y ~ x
  .. ..- attr(*, "variables")= language list(y, x)
  .. ..- attr(*, "factors")= int [1:2, 1] 0 1
  .. .. ..- attr(*, "dimnames")=List of 2
  .. .. .. ..$ : chr [1:2] "y" "x"
  .. .. .. ..$ : chr "x"
  .. ..- attr(*, "term.labels")= chr "x"
  .. ..- attr(*, "order")= int 1
  .. ..- attr(*, "intercept")= int 1
  .. ..- attr(*, "response")= int 1
  .. ..- attr(*, ".Environment")=<environment: R_GlobalEnv> 
  .. ..- attr(*, "predvars")= language list(y, x)
  .. ..- attr(*, "dataClasses")= Named chr [1:2] "numeric" "numeric"
  .. .. ..- attr(*, "names")= chr [1:2] "y" "x"
 $ residuals    : Named num [1:500] -16.12 -7.66 -16.64 4.47 -3.55 ...
  ..- attr(*, "names")= chr [1:500] "1" "2" "3" "4" ...
 $ coefficients : num [1:2, 1:4] -0.7716 0.4277 2.2726 0.0449 -0.3395 ...
  ..- attr(*, "dimnames")=List of 2
  .. ..$ : chr [1:2] "(Intercept)" "x"
  .. ..$ : chr [1:4] "Estimate" "Std. Error" "t value" "Pr(>|t|)"
 $ aliased      : Named logi [1:2] FALSE FALSE
  ..- attr(*, "names")= chr [1:2] "(Intercept)" "x"
 $ sigma        : num 9.71
 $ df           : int [1:3] 2 498 2
 $ r.squared    : num 0.154
 $ adj.r.squared: num 0.153
 $ fstatistic   : Named num [1:3] 90.8 1 498
  ..- attr(*, "names")= chr [1:3] "value" "numdf" "dendf"
 $ cov.unscaled : num [1:2, 1:2] 5.47e-02 -1.06e-03 -1.06e-03 2.13e-05
  ..- attr(*, "dimnames")=List of 2
  .. ..$ : chr [1:2] "(Intercept)" "x"
  .. ..$ : chr [1:2] "(Intercept)" "x"
 - attr(*, "class")= chr "summary.lm"
```

e nós já sabemos como indexar os elementos de essas estruturas. Temos como extrair o R^2 ajustado e o valor p para *x* com o seguinte código:


```r
> summary(modelo)$coefficients[2, 4]
```

```
[1] 6.918045e-20
```

```r
> summary(modelo)$adj.r.squared
```

```
[1] 0.1525333
```

Porém é muito mais fácil trabalhar com os resultados contidos em data frames arrumados, especialmente quando esses resultados são os argumentos para funções a serem aplicada subsequentemente. O pacote `broom` tem funções que a partir de resultados em uma estrutura complexa, produzem data frames arrumados do conteúdo principal (`tidy`), das medidas de sumarização (`glance`), e da combinação dos dados com o resultado da função aplicada aos mesmos (`augment`).  

Assim, com a função `glance` é mais simples obter os dois valores anteriores.


```r
> library(broom)
> glance(summary(modelo))
```

```
  r.squared adj.r.squared    sigma statistic      p.value df
1 0.1542317     0.1525333 9.712656  90.81373 6.918045e-20  2
```

```r
> glance(summary(modelo))[ , c(2, 5)]
```

```
  adj.r.squared      p.value
1     0.1525333 6.918045e-20
```

A utilidade da `tidy` e `augment` aplicadas em `modelo` pode ser difícil de enxergar para que não tem familiaridade com as regressões, mas a título de exemplo seguem os códigos.  


```r
> tidy(summary(modelo))
```

```
         term   estimate  std.error  statistic      p.value
1 (Intercept) -0.7716281 2.27261907 -0.3395325 7.343517e-01
2           x  0.4276520 0.04487607  9.5296237 6.918045e-20
```


```r
> head(augment(modelo))
```

```
          y        x  .fitted   .se.fit      .resid        .hat
1  5.420216 52.16755 21.53793 0.4481616 -16.1177145 0.002129086
2 10.630271 44.57507 18.29099 0.4916720  -7.6607229 0.002562565
3  7.782182 58.91145 24.42197 0.5993604 -16.6397901 0.003808027
4 27.625572 55.95981 23.15970 0.5170784   4.4658748 0.002834241
5 24.054606 66.35618 27.60573 0.8641769  -3.5511216 0.007916428
6 23.233784 56.89275 23.55867 0.5409372  -0.3248908 0.003101828
    .sigma      .cooksd  .std.resid
1 9.695447 2.944051e-03 -1.66122423
2 9.716332 8.011928e-04 -0.78974867
3 9.693620 5.631234e-03 -1.71647825
4 9.720353 3.013069e-04  0.46045251
5 9.721107 5.375982e-04 -0.36707380
6 9.722412 1.746164e-06 -0.03350225
```
