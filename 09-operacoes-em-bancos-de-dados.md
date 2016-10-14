

## Operações em bancos de dados

Em capítulos anteriores aplicamos funções em data frames que podem ser entendidas como operações em bancos de dados. Neste capítulo daremos continuidade às operações de bancos de dados sob uma abordagem mais eficiente, consistente e sofisticada implementada no pacote `dplyr`, que além de oferecer funções para operar com um ou dois data frames, suporta operações encadeadas e uma abordagem conhecida como *partição, aplicação, e combinação*.  

Várias das operações que veremos são realizáveis com alternativas que prescinde do `dplyr`. Entretanto, com este último a execução usualmente é mais rápida (relevante com grandes bancos de dados) e consistente. 

### Operações em um data frame

#### Seleção de linhas

A função `filter` seleciona linhas do banco que recebe como primeiro argumento, de acordo com uma ou mais condições especificadas nos outros argumentos.


```r
> set.seed(3)
> (domicilios <- data.frame(municipio = rep(c('a1', 'b', 'c'), e = 10),
+                          id = 1:30,
+                          caes = rpois(30, .95),
+                          gatos = rpois(30, .6),
+                          stringsAsFactors = F))
```

```
   municipio id caes gatos
1         a1  1    0     0
2         a1  2    2     0
3         a1  3    0     0
4         a1  4    0     0
5         a1  5    1     0
6         a1  6    1     0
7         a1  7    0     2
8         a1  8    0     0
9         a1  9    1     1
10        a1 10    1     0
11         b 11    1     0
12         b 12    1     1
13         b 13    1     0
14         b 14    1     1
15         b 15    2     0
16         b 16    2     0
17         b 17    0     0
18         b 18    1     0
19         b 19    2     0
20         b 20    0     1
21         c 21    0     0
22         c 22    0     0
23         c 23    0     1
24         c 24    0     3
25         c 25    0     1
26         c 26    2     2
27         c 27    1     0
28         c 28    2     0
29         c 29    1     0
30         c 30    2     0
```

Reparem que com as funções do `dplyr` os nomes das colunas não estão contidos em aspas.


```r
> library(dplyr)
> filter(domicilios, caes > 1)
```

```
  municipio id caes gatos
1        a1  2    2     0
2         b 15    2     0
3         b 16    2     0
4         b 19    2     0
5         c 26    2     2
6         c 28    2     0
7         c 30    2     0
```


```r
> filter(domicilios, caes == 1, gatos == 1)
```

```
  municipio id caes gatos
1        a1  9    1     1
2         b 12    1     1
3         b 14    1     1
```

#### Ordenação das linhas

A função `arrange` ordena o banco com base em uma ou mais colunas. Ao aplicarmos a função `desc` nas colunas, a ordenação e descendente.


```r
> arrange(domicilios, caes)
```

```
   municipio id caes gatos
1         a1  1    0     0
2         a1  3    0     0
3         a1  4    0     0
4         a1  7    0     2
5         a1  8    0     0
6          b 17    0     0
7          b 20    0     1
8          c 21    0     0
9          c 22    0     0
10         c 23    0     1
11         c 24    0     3
12         c 25    0     1
13        a1  5    1     0
14        a1  6    1     0
15        a1  9    1     1
16        a1 10    1     0
17         b 11    1     0
18         b 12    1     1
19         b 13    1     0
20         b 14    1     1
21         b 18    1     0
22         c 27    1     0
23         c 29    1     0
24        a1  2    2     0
25         b 15    2     0
26         b 16    2     0
27         b 19    2     0
28         c 26    2     2
29         c 28    2     0
30         c 30    2     0
```


```r
> arrange(domicilios, desc(caes))
```

```
   municipio id caes gatos
1         a1  2    2     0
2          b 15    2     0
3          b 16    2     0
4          b 19    2     0
5          c 26    2     2
6          c 28    2     0
7          c 30    2     0
8         a1  5    1     0
9         a1  6    1     0
10        a1  9    1     1
11        a1 10    1     0
12         b 11    1     0
13         b 12    1     1
14         b 13    1     0
15         b 14    1     1
16         b 18    1     0
17         c 27    1     0
18         c 29    1     0
19        a1  1    0     0
20        a1  3    0     0
21        a1  4    0     0
22        a1  7    0     2
23        a1  8    0     0
24         b 17    0     0
25         b 20    0     1
26         c 21    0     0
27         c 22    0     0
28         c 23    0     1
29         c 24    0     3
30         c 25    0     1
```


```r
> arrange(domicilios, caes, gatos)
```

```
   municipio id caes gatos
1         a1  1    0     0
2         a1  3    0     0
3         a1  4    0     0
4         a1  8    0     0
5          b 17    0     0
6          c 21    0     0
7          c 22    0     0
8          b 20    0     1
9          c 23    0     1
10         c 25    0     1
11        a1  7    0     2
12         c 24    0     3
13        a1  5    1     0
14        a1  6    1     0
15        a1 10    1     0
16         b 11    1     0
17         b 13    1     0
18         b 18    1     0
19         c 27    1     0
20         c 29    1     0
21        a1  9    1     1
22         b 12    1     1
23         b 14    1     1
24        a1  2    2     0
25         b 15    2     0
26         b 16    2     0
27         b 19    2     0
28         c 28    2     0
29         c 30    2     0
30         c 26    2     2
```


```r
> arrange(domicilios, desc(caes), gatos)
```

```
   municipio id caes gatos
1         a1  2    2     0
2          b 15    2     0
3          b 16    2     0
4          b 19    2     0
5          c 28    2     0
6          c 30    2     0
7          c 26    2     2
8         a1  5    1     0
9         a1  6    1     0
10        a1 10    1     0
11         b 11    1     0
12         b 13    1     0
13         b 18    1     0
14         c 27    1     0
15         c 29    1     0
16        a1  9    1     1
17         b 12    1     1
18         b 14    1     1
19        a1  1    0     0
20        a1  3    0     0
21        a1  4    0     0
22        a1  8    0     0
23         b 17    0     0
24         c 21    0     0
25         c 22    0     0
26         b 20    0     1
27         c 23    0     1
28         c 25    0     1
29        a1  7    0     2
30         c 24    0     3
```

#### Seleção de colunas 

No capítulo *Objetos* vimos que para selecionar colunas de um data frame tínhamos que usar um vetor com as posições das colunas ou com os nomes entre aspas. O `dplyr` é mais flexível na hora de selecionar colunas.


```r
> head(select(domicilios, 1:3))
```

```
  municipio id caes
1        a1  1    0
2        a1  2    2
3        a1  3    0
4        a1  4    0
5        a1  5    1
6        a1  6    1
```

Nomes sem aspas.


```r
> head(select(domicilios, c(municipio, id, caes)))
```

```
  municipio id caes
1        a1  1    0
2        a1  2    2
3        a1  3    0
4        a1  4    0
5        a1  5    1
6        a1  6    1
```

Podemos colocar as colunas como argumentos diferentes


```r
> head(select(domicilios, municipio, id, gatos))
```

```
  municipio id gatos
1        a1  1     0
2        a1  2     0
3        a1  3     0
4        a1  4     0
5        a1  5     0
6        a1  6     0
```


```r
> head(select(domicilios, 1, 2, 3))
```

```
  municipio id caes
1        a1  1    0
2        a1  2    2
3        a1  3    0
4        a1  4    0
5        a1  5    1
6        a1  6    1
```

e criar sequências de colunas não apenas com as posições (`1:3`), mas também com os nomes (`municipio:caes`).


```r
> head(select(domicilios, municipio:caes))
```

```
  municipio id caes
1        a1  1    0
2        a1  2    2
3        a1  3    0
4        a1  4    0
5        a1  5    1
6        a1  6    1
```

O sinal `-` omite colunas.


```r
> head(select(domicilios, -municipio))
```

```
  id caes gatos
1  1    0     0
2  2    2     0
3  3    0     0
4  4    0     0
5  5    1     0
6  6    1     0
```


```r
> head(select(domicilios, -municipio, -gatos))
```

```
  id caes
1  1    0
2  2    2
3  3    0
4  4    0
5  5    1
6  6    1
```


```r
> head(select(domicilios, -c(municipio:caes)))
```

```
  gatos
1     0
2     0
3     0
4     0
5     0
6     0
```

Para selecionar com base em um vetor de caracteres, devemos usar a função `one_of`.


```r
> head(select(domicilios, one_of(c('municipio', 'id', 'caes'))))
```

```
  municipio id caes
1        a1  1    0
2        a1  2    2
3        a1  3    0
4        a1  4    0
5        a1  5    1
6        a1  6    1
```

Há um conjunto de funções que servem para selecionar colunas cujo nome começa, termina ou contém um dado caractere.


```r
> head(select(domicilios, starts_with('ca')))
```

```
  caes
1    0
2    2
3    0
4    0
5    1
6    1
```


```r
> head(select(domicilios, ends_with('s')))
```

```
  caes gatos
1    0     0
2    2     0
3    0     0
4    0     0
5    1     0
6    1     0
```


```r
> head(select(domicilios, contains('a')))
```

```
  caes gatos
1    0     0
2    2     0
3    0     0
4    0     0
5    1     0
6    1     0
```

Quando os nomes das colunas têm um prefixo seguido por um número, podemos especificar o prefixo e uma sequência de números.


```r
> (banco_numerico <- as.data.frame(matrix(runif(50, 10, 20), ncol = 5)))
```

```
         V1       V2       V3       V4       V5
1  18.16106 17.59755 17.84215 10.11744 17.66338
2  10.57613 17.60820 16.54355 12.67403 16.82132
3  18.02829 19.03261 13.78104 14.35572 12.09131
4  11.04388 19.66283 10.08567 18.29467 17.11944
5  17.66606 15.15257 19.55330 18.70944 16.05298
6  13.04811 15.49481 18.38616 12.51069 13.40559
7  17.69287 11.63720 12.13425 13.24358 10.41170
8  15.40656 11.64597 14.94714 13.06238 14.01753
9  13.62371 17.86333 16.36244 11.84282 10.79060
10 10.92557 17.51113 19.21091 16.79977 13.12553
```


```r
> head(select(banco_numerico, num_range(prefix = 'V', 3:5)))
```

```
        V3       V4       V5
1 17.84215 10.11744 17.66338
2 16.54355 12.67403 16.82132
3 13.78104 14.35572 12.09131
4 10.08567 18.29467 17.11944
5 19.55330 18.70944 16.05298
6 18.38616 12.51069 13.40559
```

Com `select_if` selecionamos só as colunas que satisfazem uma dada condição.


```r
> head(select_if(domicilios, is.character))
```

```
  municipio
1        a1
2        a1
3        a1
4        a1
5        a1
6        a1
```

#### Renomeação

A função `rename` renomeia usando a sintaxe `novo_nome = nome_atual`.


```r
> head(rename(domicilios, city = municipio, dogs = caes, cats = gatos))
```

```
  city id dogs cats
1   a1  1    0    0
2   a1  2    2    0
3   a1  3    0    0
4   a1  4    0    0
5   a1  5    1    0
6   a1  6    1    0
```

#### Remoção de linhas duplicadas

Modifiquemos o banco `domicilios` para que fique com três repetições da primeira linha e duas da terceira.


```r
> head(domicilios <- rbind(domicilios[c(1, 1, 3), ], domicilios))
```

```
    municipio id caes gatos
1          a1  1    0     0
1.1        a1  1    0     0
3          a1  3    0     0
110        a1  1    0     0
2          a1  2    2     0
31         a1  3    0     0
```

A função `distinct` remove as linhas duplicadas.


```r
> distinct(domicilios)
```

```
   municipio id caes gatos
1         a1  1    0     0
2         a1  3    0     0
3         a1  2    2     0
4         a1  4    0     0
5         a1  5    1     0
6         a1  6    1     0
7         a1  7    0     2
8         a1  8    0     0
9         a1  9    1     1
10        a1 10    1     0
11         b 11    1     0
12         b 12    1     1
13         b 13    1     0
14         b 14    1     1
15         b 15    2     0
16         b 16    2     0
17         b 17    0     0
18         b 18    1     0
19         b 19    2     0
20         b 20    0     1
21         c 21    0     0
22         c 22    0     0
23         c 23    0     1
24         c 24    0     3
25         c 25    0     1
26         c 26    2     2
27         c 27    1     0
28         c 28    2     0
29         c 29    1     0
30         c 30    2     0
```

Se especificamos uma ou mais colunas, as duplicatas são consideradas só nessas colunas.


```r
> distinct(domicilios, municipio)
```

```
  municipio
1        a1
2         b
3         c
```


```r
> distinct(domicilios, municipio, caes)
```

```
  municipio caes
1        a1    0
2        a1    2
3        a1    1
4         b    1
5         b    2
6         b    0
7         c    0
8         c    2
9         c    1
```

Deixemos o banco como estava.


```r
> domicilios <- distinct(domicilios)
```

#### Transformação e adição de colunas

Com `mutate` podemos transformar e adicionar colunas definido ou redefinindo as colunas de interese. 


```r
> mutate(domicilios,
+        municipio = rep(1:3, e = 10),
+        id = id + 100,
+        animais = caes + gatos)
```

```
   municipio  id caes gatos animais
1          1 101    0     0       0
2          1 103    0     0       0
3          1 102    2     0       2
4          1 104    0     0       0
5          1 105    1     0       1
6          1 106    1     0       1
7          1 107    0     2       2
8          1 108    0     0       0
9          1 109    1     1       2
10         1 110    1     0       1
11         2 111    1     0       1
12         2 112    1     1       2
13         2 113    1     0       1
14         2 114    1     1       2
15         2 115    2     0       2
16         2 116    2     0       2
17         2 117    0     0       0
18         2 118    1     0       1
19         2 119    2     0       2
20         2 120    0     1       1
21         3 121    0     0       0
22         3 122    0     0       0
23         3 123    0     1       1
24         3 124    0     3       3
25         3 125    0     1       1
26         3 126    2     2       4
27         3 127    1     0       1
28         3 128    2     0       2
29         3 129    1     0       1
30         3 130    2     0       2
```

`transmutate` segue a mesma lógica de `mutate`, porém, só as colunas transformadas ou adicionadas são retornadas.


```r
> transmute(domicilios,
+           municipio = rep(1:3, e = 10),
+           id = id + 100,
+           animais = caes + gatos)
```

```
   municipio  id animais
1          1 101       0
2          1 103       0
3          1 102       2
4          1 104       0
5          1 105       1
6          1 106       1
7          1 107       2
8          1 108       0
9          1 109       2
10         1 110       1
11         2 111       1
12         2 112       2
13         2 113       1
14         2 114       2
15         2 115       2
16         2 116       2
17         2 117       0
18         2 118       1
19         2 119       2
20         2 120       1
21         3 121       0
22         3 122       0
23         3 123       1
24         3 124       3
25         3 125       1
26         3 126       4
27         3 127       1
28         3 128       2
29         3 129       1
30         3 130       2
```

`mutate_all` aplica uma função a todas as colunas,


```r
> mutate_all(banco_numerico, log)
```

```
         V1       V2       V3       V4       V5
1  2.899280 2.867760 2.881563 2.314261 2.871493
2  2.358600 2.868365 2.805996 2.539555 2.822647
3  2.891942 2.946154 2.623294 2.664149 2.492487
4  2.401876 2.978730 2.311116 2.906610 2.840215
5  2.871645 2.718170 2.973144 2.929028 2.775895
6  2.568643 2.740505 2.911598 2.526583 2.595672
7  2.873162 2.454207 2.496032 2.583513 2.342930
8  2.734793 2.454960 2.704520 2.569736 2.640308
9  2.611811 2.882750 2.794989 2.471722 2.378675
10 2.391106 2.862837 2.955479 2.821365 2.574559
```

enquanto `mutate_at` aplica a função só às funções especificadas. Porém, `mutate_at` só aceita a especificação de sequência de nomes com a função `vars` (`vars(V1:V3)` no lugar de `V1:V3`).


```r
> mutate_at(banco_numerico, 1:3, mean)
```

```
         V1       V2       V3       V4       V5
1  14.61722 16.32062 15.88466 10.11744 17.66338
2  14.61722 16.32062 15.88466 12.67403 16.82132
3  14.61722 16.32062 15.88466 14.35572 12.09131
4  14.61722 16.32062 15.88466 18.29467 17.11944
5  14.61722 16.32062 15.88466 18.70944 16.05298
6  14.61722 16.32062 15.88466 12.51069 13.40559
7  14.61722 16.32062 15.88466 13.24358 10.41170
8  14.61722 16.32062 15.88466 13.06238 14.01753
9  14.61722 16.32062 15.88466 11.84282 10.79060
10 14.61722 16.32062 15.88466 16.79977 13.12553
```


```r
> mutate_at(banco_numerico, c('V1', 'V2', 'V3'), mean)
```

```
         V1       V2       V3       V4       V5
1  14.61722 16.32062 15.88466 10.11744 17.66338
2  14.61722 16.32062 15.88466 12.67403 16.82132
3  14.61722 16.32062 15.88466 14.35572 12.09131
4  14.61722 16.32062 15.88466 18.29467 17.11944
5  14.61722 16.32062 15.88466 18.70944 16.05298
6  14.61722 16.32062 15.88466 12.51069 13.40559
7  14.61722 16.32062 15.88466 13.24358 10.41170
8  14.61722 16.32062 15.88466 13.06238 14.01753
9  14.61722 16.32062 15.88466 11.84282 10.79060
10 14.61722 16.32062 15.88466 16.79977 13.12553
```


```r
> mutate_at(banco_numerico, vars(V1:V3), mean)
```

```
         V1       V2       V3       V4       V5
1  14.61722 16.32062 15.88466 10.11744 17.66338
2  14.61722 16.32062 15.88466 12.67403 16.82132
3  14.61722 16.32062 15.88466 14.35572 12.09131
4  14.61722 16.32062 15.88466 18.29467 17.11944
5  14.61722 16.32062 15.88466 18.70944 16.05298
6  14.61722 16.32062 15.88466 12.51069 13.40559
7  14.61722 16.32062 15.88466 13.24358 10.41170
8  14.61722 16.32062 15.88466 13.06238 14.01753
9  14.61722 16.32062 15.88466 11.84282 10.79060
10 14.61722 16.32062 15.88466 16.79977 13.12553
```

Com `mutate_if` transformamos só as colunas que satisfazem uma dada condição. A condição deve ser especificada no segundo argumento e a função a ser aplicada no terceiro.


```r
> mutate_if(domicilios, is.character, toupper)
```

```
   municipio id caes gatos
1         A1  1    0     0
2         A1  3    0     0
3         A1  2    2     0
4         A1  4    0     0
5         A1  5    1     0
6         A1  6    1     0
7         A1  7    0     2
8         A1  8    0     0
9         A1  9    1     1
10        A1 10    1     0
11         B 11    1     0
12         B 12    1     1
13         B 13    1     0
14         B 14    1     1
15         B 15    2     0
16         B 16    2     0
17         B 17    0     0
18         B 18    1     0
19         B 19    2     0
20         B 20    0     1
21         C 21    0     0
22         C 22    0     0
23         C 23    0     1
24         C 24    0     3
25         C 25    0     1
26         C 26    2     2
27         C 27    1     0
28         C 28    2     0
29         C 29    1     0
30         C 30    2     0
```

#### Sumarização

Podemos sumarizar uma ou mais colunas com `summarise` e suas variações. A sumarização é a aplicação de funções que a partir de uma coluna retornam um único valor.


```r
> summarise(domicilios, total_caes = sum(caes), media_gatos = mean(gatos))
```

```
  total_caes media_gatos
1         25   0.4333333
```


```r
> summarise_all(banco_numerico, sum)
```

```
        V1       V2       V3       V4       V5
1 146.1722 163.2062 158.8466 141.6105 141.4994
```


```r
> summarise_at(banco_numerico, vars(V1:V3), mean)
```

```
        V1       V2       V3
1 14.61722 16.32062 15.88466
```


```r
> summarise_if(domicilios, is.numeric, max)
```

```
  id caes gatos
1 30    2     3
```

#### Amostragem de colunas

`sample_n` e `sample_frac` amostram um número ou fração de linhas respectivamente.


```r
> set.seed(7)
> sample_n(domicilios, 5)
```

```
   municipio id caes gatos
30         c 30    2     0
12         b 12    1     1
4         a1  4    0     0
2         a1  3    0     0
7         a1  7    0     2
```


```r
> set.seed(7)
> sample_n(domicilios, 5, replace = T)
```

```
   municipio id caes gatos
30         c 30    2     0
12         b 12    1     1
4         a1  4    0     0
3         a1  2    2     0
8         a1  8    0     0
```


```r
> set.seed(7)
> sample_frac(domicilios, .1)
```

```
   municipio id caes gatos
30         c 30    2     0
12         b 12    1     1
4         a1  4    0     0
```

### Operações com dois data frames

Criemos mais um banco para explorar as funções que operam em dois data frames.


```r
> set.seed(5)
> (municipios <- data.frame(municipio = letters[1:10],
+                          idh = runif(10, .7, .8),
+                          stringsAsFactors = F))
```

```
   municipio       idh
1          a 0.7200214
2          b 0.7685219
3          c 0.7916876
4          d 0.7284399
5          e 0.7104650
6          f 0.7701057
7          g 0.7527960
8          h 0.7807935
9          i 0.7956500
10         j 0.7110453
```

#### Junção interna

Com base em uma ou mais colunas (no caso `municipio`), `inner_join` retorna todas as linhas de `domicílios` que também estão em `municipios`, e todas as colunas de ambos bancos.


```r
> inner_join(domicilios, municipios, by = 'municipio')
```

```
   municipio id caes gatos       idh
1          b 11    1     0 0.7685219
2          b 12    1     1 0.7685219
3          b 13    1     0 0.7685219
4          b 14    1     1 0.7685219
5          b 15    2     0 0.7685219
6          b 16    2     0 0.7685219
7          b 17    0     0 0.7685219
8          b 18    1     0 0.7685219
9          b 19    2     0 0.7685219
10         b 20    0     1 0.7685219
11         c 21    0     0 0.7916876
12         c 22    0     0 0.7916876
13         c 23    0     1 0.7916876
14         c 24    0     3 0.7916876
15         c 25    0     1 0.7916876
16         c 26    2     2 0.7916876
17         c 27    1     0 0.7916876
18         c 28    2     0 0.7916876
19         c 29    1     0 0.7916876
20         c 30    2     0 0.7916876
```

#### Junção esquerda

Com base em uma ou mais colunas (no caso `municipio`), `left_join` retorna todas as linhas de `domicilios`, e todas as colunas de `domicilios` e `municipios`. As linhas de `domicilios` que não estão em `municipios` assumem o valor `NA` nas colunas de `municipios`.


```r
> left_join(domicilios, municipios, by = 'municipio')
```

```
   municipio id caes gatos       idh
1         a1  1    0     0        NA
2         a1  3    0     0        NA
3         a1  2    2     0        NA
4         a1  4    0     0        NA
5         a1  5    1     0        NA
6         a1  6    1     0        NA
7         a1  7    0     2        NA
8         a1  8    0     0        NA
9         a1  9    1     1        NA
10        a1 10    1     0        NA
11         b 11    1     0 0.7685219
12         b 12    1     1 0.7685219
13         b 13    1     0 0.7685219
14         b 14    1     1 0.7685219
15         b 15    2     0 0.7685219
16         b 16    2     0 0.7685219
17         b 17    0     0 0.7685219
18         b 18    1     0 0.7685219
19         b 19    2     0 0.7685219
20         b 20    0     1 0.7685219
21         c 21    0     0 0.7916876
22         c 22    0     0 0.7916876
23         c 23    0     1 0.7916876
24         c 24    0     3 0.7916876
25         c 25    0     1 0.7916876
26         c 26    2     2 0.7916876
27         c 27    1     0 0.7916876
28         c 28    2     0 0.7916876
29         c 29    1     0 0.7916876
30         c 30    2     0 0.7916876
```

#### Junção direita

Com base em uma ou mais colunas (no caso `municipio`), `right_join` retorna todas as linhas de `municipios`, e todas as colunas de `domicilios` e `municipios`. As linhas de `municipios` que não estão em `domicilios` assumem o valor `NA` nas colunas de `domicilios`.


```r
> right_join(domicilios, municipios, by = 'municipio')
```

```
   municipio id caes gatos       idh
1          a NA   NA    NA 0.7200214
2          b 11    1     0 0.7685219
3          b 12    1     1 0.7685219
4          b 13    1     0 0.7685219
5          b 14    1     1 0.7685219
6          b 15    2     0 0.7685219
7          b 16    2     0 0.7685219
8          b 17    0     0 0.7685219
9          b 18    1     0 0.7685219
10         b 19    2     0 0.7685219
11         b 20    0     1 0.7685219
12         c 21    0     0 0.7916876
13         c 22    0     0 0.7916876
14         c 23    0     1 0.7916876
15         c 24    0     3 0.7916876
16         c 25    0     1 0.7916876
17         c 26    2     2 0.7916876
18         c 27    1     0 0.7916876
19         c 28    2     0 0.7916876
20         c 29    1     0 0.7916876
21         c 30    2     0 0.7916876
22         d NA   NA    NA 0.7284399
23         e NA   NA    NA 0.7104650
24         f NA   NA    NA 0.7701057
25         g NA   NA    NA 0.7527960
26         h NA   NA    NA 0.7807935
27         i NA   NA    NA 0.7956500
28         j NA   NA    NA 0.7110453
```

#### Junção completa

Com base em uma ou mais colunas (no caso `municipio`), `full_join` retorna todas as linhas e as colunas de `domicilios` e `municipios`. As linhas de `domicilios` que não estão em `municipios` assumem o valor `NA` nas colunas de `municipios` (e vice versa).


```r
> full_join(domicilios, municipios, by = 'municipio')
```

```
   municipio id caes gatos       idh
1         a1  1    0     0        NA
2         a1  3    0     0        NA
3         a1  2    2     0        NA
4         a1  4    0     0        NA
5         a1  5    1     0        NA
6         a1  6    1     0        NA
7         a1  7    0     2        NA
8         a1  8    0     0        NA
9         a1  9    1     1        NA
10        a1 10    1     0        NA
11         b 11    1     0 0.7685219
12         b 12    1     1 0.7685219
13         b 13    1     0 0.7685219
14         b 14    1     1 0.7685219
15         b 15    2     0 0.7685219
16         b 16    2     0 0.7685219
17         b 17    0     0 0.7685219
18         b 18    1     0 0.7685219
19         b 19    2     0 0.7685219
20         b 20    0     1 0.7685219
21         c 21    0     0 0.7916876
22         c 22    0     0 0.7916876
23         c 23    0     1 0.7916876
24         c 24    0     3 0.7916876
25         c 25    0     1 0.7916876
26         c 26    2     2 0.7916876
27         c 27    1     0 0.7916876
28         c 28    2     0 0.7916876
29         c 29    1     0 0.7916876
30         c 30    2     0 0.7916876
31         a NA   NA    NA 0.7200214
32         d NA   NA    NA 0.7284399
33         e NA   NA    NA 0.7104650
34         f NA   NA    NA 0.7701057
35         g NA   NA    NA 0.7527960
36         h NA   NA    NA 0.7807935
37         i NA   NA    NA 0.7956500
38         j NA   NA    NA 0.7110453
```

#### Anti junção

Com base em uma ou mais colunas (no caso `municipio`), `anti_join` retorna todas as linhas de `domicilios` que não estão em `municipios`, e todas as colunas de `domicilios`.


```r
> anti_join(domicilios, municipios, by = 'municipio')
```

```
   municipio id caes gatos
1         a1  1    0     0
2         a1  3    0     0
3         a1  2    2     0
4         a1  4    0     0
5         a1  5    1     0
6         a1  6    1     0
7         a1  7    0     2
8         a1  8    0     0
9         a1  9    1     1
10        a1 10    1     0
```

### Partição, aplicação e combinação

Na abordagem partição, aplicação e combinação consiste na partição de um banco de dados de acordo com os valores de uma ou mais colunas, a aplicação de operações a cada uma das partições de forma independente, e na combinação dos resultados das operações em um novo banco.  

Por exemplo, podemos particionar o banco `domicilios` com base na coluna `municipio` que por ter três valores únicos (`a1`, `b`, e `c`), resultará em três partições (`a1`, `b`, e `c`). Em cada partição podemos calcular a média de cães e o total de gatos, e posteriormente criar um banco da média de cães e do total de gatos por município. A partição é mediada pela função `group_by`, enquanto os cálculos da média e do total são operações de sumarização (`summarise` em cada um dos grupos).


```r
> domicilios %>%
+   group_by(municipio) %>%
+   summarise(media_caes = mean(caes), total_gatos = sum(gatos))
```

```
# A tibble: 3 × 3
  municipio media_caes total_gatos
      <chr>      <dbl>       <int>
1        a1        0.6           3
2         b        1.1           3
3         c        0.8           7
```

Quando aplicamos a função `group_by`, a maioria das funções subsequentes são aplicadas dentro de cada grupo. Portanto, se particionamos, depois amostramos e finalmente sumarizamos, o banco resultante terá tantas linhas como grupos, como no caso anterior.


```r
> domicilios %>%
+   group_by(municipio) %>%
+   sample_n(5) %>%
+   summarise(media_caes = mean(caes), media_gatos = mean(gatos))
```

```
# A tibble: 3 × 3
  municipio media_caes media_gatos
      <chr>      <dbl>       <dbl>
1        a1        0.8         0.0
2         b        1.4         0.2
3         c        0.8         0.0
```

Em ocasiões é necessário aplicar funções separadamente nas partições, para depois aplicar funções no total de observações. Nesses casos devemos desagrupar o banco com `ungroup` antes de aplicar as funções no total das observações.


```r
> domicilios %>%
+   group_by(municipio) %>%
+   sample_n(5) %>%
+   ungroup() %>%
+   summarise(media_caes = mean(caes), media_gatos = mean(gatos))
```

```
# A tibble: 1 × 2
  media_caes media_gatos
       <dbl>       <dbl>
1  0.8666667   0.2666667
```

A função `group_by` atribui novas *classes* ao objeto resultante e uma das consequências disso é que o padrão de apresentação do data frame resultante é diferente: o tipo de cada coluna aparece abaixo do nome (`<chr>`: caractere, `<int>`: inteiro, etc.) e só as 10 primeiras linhas são mostradas. Para obtermos a apresentação padrão dos data frames, devemos coercionar para essa estrutura.


```r
> (doms <- domicilios %>%
+   group_by(municipio) %>%
+   mutate(prop_caes = caes / sum(caes)))
```

```
Source: local data frame [30 x 5]
Groups: municipio [3]

   municipio    id  caes gatos prop_caes
       <chr> <int> <int> <int>     <dbl>
1         a1     1     0     0 0.0000000
2         a1     3     0     0 0.0000000
3         a1     2     2     0 0.3333333
4         a1     4     0     0 0.0000000
5         a1     5     1     0 0.1666667
6         a1     6     1     0 0.1666667
7         a1     7     0     2 0.0000000
8         a1     8     0     0 0.0000000
9         a1     9     1     1 0.1666667
10        a1    10     1     0 0.1666667
# ... with 20 more rows
```

```r
> as.data.frame(doms)
```

```
   municipio id caes gatos  prop_caes
1         a1  1    0     0 0.00000000
2         a1  3    0     0 0.00000000
3         a1  2    2     0 0.33333333
4         a1  4    0     0 0.00000000
5         a1  5    1     0 0.16666667
6         a1  6    1     0 0.16666667
7         a1  7    0     2 0.00000000
8         a1  8    0     0 0.00000000
9         a1  9    1     1 0.16666667
10        a1 10    1     0 0.16666667
11         b 11    1     0 0.09090909
12         b 12    1     1 0.09090909
13         b 13    1     0 0.09090909
14         b 14    1     1 0.09090909
15         b 15    2     0 0.18181818
16         b 16    2     0 0.18181818
17         b 17    0     0 0.00000000
18         b 18    1     0 0.09090909
19         b 19    2     0 0.18181818
20         b 20    0     1 0.00000000
21         c 21    0     0 0.00000000
22         c 22    0     0 0.00000000
23         c 23    0     1 0.00000000
24         c 24    0     3 0.00000000
25         c 25    0     1 0.00000000
26         c 26    2     2 0.25000000
27         c 27    1     0 0.12500000
28         c 28    2     0 0.25000000
29         c 29    1     0 0.12500000
30         c 30    2     0 0.25000000
```

### Outras operações

O banco `reg` tem 500 observações com as variáveis `y` e `x`. Cada observação pertence a um de dez grupos e isso é indicado pela variável `grupo`.


```r
> set.seed(4)
> x <- rnorm(500, 50, 10)
> reg <- data.frame(grupo = rep(1:10, e = 50),
+                     y = 1 + 0.4 * x + rnorm(500, 0, 10),
+                     x = x)
```

Se queremos ajustar uma regressão linear dentro de cada grupo, as funções do `dplyr` vistas até o momento não são suficientes. Entretanto, a função `do` permite aplicar operações arbitrárias e retorna um data frame. Por exemplo, podemos aplicar a função `head` dentro de cada grupo.


```r
> reg %>%
+   group_by(grupo) %>%
+   do(head(.))
```

```
Source: local data frame [60 x 3]
Groups: grupo [10]

   grupo         y        x
   <int>     <dbl>    <dbl>
1      1  5.420216 52.16755
2      1 10.630271 44.57507
3      1  7.782182 58.91145
4      1 27.625572 55.95981
5      1 24.054606 66.35618
6      1 23.233784 56.89275
7      2  9.477180 43.36257
8      2 24.875976 43.76274
9      2 35.672370 49.20368
10     2  7.730713 54.35625
# ... with 50 more rows
```

Reparem que no lugar de `do(head)` a sintaxe é `do(head(.))`. `.` é um *placeholder* que representa cada um dos grupos. Dentro de `do` podemos aplicar várias funções se as mesmas estão contidas em chaves. Por exemplo, podemos ajustar uma regressão linear dentro de cada grupo como `mod = lm(y ~ x, data = .)` e extrair as medidas de sumarização do modelo com `glance(mod)`.


```r
> library(broom)
> reg %>%
+   group_by(grupo) %>%
+   do({mod = lm(y ~ x, data = .) %>%
+       glance(mod)}) %>%
+   select(adj.r.squared, p.value)
```

```
Adding missing grouping variables: `grupo`
```

```
Source: local data frame [10 x 3]
Groups: grupo [10]

   grupo adj.r.squared      p.value
   <int>         <dbl>        <dbl>
1      1    0.30580219 1.863413e-05
2      2    0.13891126 4.463162e-03
3      3    0.27526672 5.469821e-05
4      4    0.19286524 8.358439e-04
5      5    0.13704449 4.724054e-03
6      6    0.14038104 4.267737e-03
7      7    0.05742603 5.158791e-02
8      8    0.04843200 6.770084e-02
9      9    0.06243191 4.437932e-02
10    10    0.11335589 9.666115e-03
```
