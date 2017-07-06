---
title: "problem_one"
author: "Gustavo Monteiro"
date: "2017-05-17 21:29:54"
published: yes
tags:
- htmlwidgets
- r
layout: post
---

```{r include=FALSE}
#Carregamento das bibliotecas necessarias e dos dados.
library("tidyverse", warn.conflicts = FALSE)
data = read_csv("./data.csv")
```

Foram escolhidas as Series:
  1) 13 Reasons Why
  2) Black Mirror
  3) Game of Thrones
  4) The Walking Dead

As duas primeiras foram escolhidas por terem alcancado sucesso mais recentemente enquanto as duas outras por serem series que ja tem alguns anos de exibicao.


Filtragem das series escolhidas e agrupamento das observacoes pelo nome da serie.
```{r}
series = data %>% 
  filter(series_name %in% c("13 Reasons Why", "The Walking Dead", "Game of Thrones", "Black Mirror")) %>%
  group_by(series_name)
```

PERGUNTA A:
  "Qual das series que voce escolheu eh mais bem avaliada no IMDB? A diferen?a eh grande? Pequena?"

Teste de media:
```{r}
series %>%
  summarise(UserRating = mean(UserRating)) %>%
  ggplot(aes(x = reorder(series_name, UserRating), y = UserRating, fill = series_name)) + 
  geom_col(width = 0.7) + 
  scale_y_continuous(breaks=seq(0, 10, 1)) +
  coord_flip()
```

Foi observado que a serie 'Game of Thrones' alcancou uma media de avaliacao maior que todas as outras.

Teste de mediana:
```{r}
series %>%
  summarise(UserRating = median(UserRating, na.rm = TRUE)) %>%
  ggplot(aes(x = reorder(series_name, UserRating), y = UserRating, fill = series_name)) + 
  geom_col(width = 0.7) +
  scale_y_continuous(breaks=seq(0, 10, 1)) +
  coord_flip()
```

'Game of Thrones' tambem alcancou uma mediana ligeiramente maior que as outras series.

Box Plot:
```{r}
series %>%
  ggplot(aes(x = series_name, y = UserRating, fill = series_name)) +
  geom_boxplot()
```

Mais uma vez 'Game of Thrones' lidera, tendo o segundo quartil dos das avaliacoes de dos seus episodios com uma avaliacao melhor que o terceiro quartil dos episodios das outras tres series.

RESPOSTA:
  Dadas as observacoes se conclui que 'Game of Thrones' ? a mais bem avaliada no IMDB dentre as quatro series escolhidas.
  A diferenca pode ser considerada pequena levando em conta os testes de media e mediana que em ambos a diferenca entre o primeiro e o ultimo colocado ? inferior a 1 ponto, que mesmo com com o box plot mostrando de forma mais explica uma melhor avalicao de 'Game of Thrones' as diferencas ainda sao bem inferiores a 1 ponto.

---------------------------------------------------------------------------------

PERGUNTA B:
  "Qual das series que voce escolheu tem episodios de qualidade mais irregular segundo o IMDB? A diferenca eh grande? Pequena?"
  
Teste do desvio padrao:
```{r}
series %>%
  summarise(UserRating = sd(UserRating)) %>%
  ggplot(aes(x = reorder(series_name, UserRating), y = UserRating, fill = series_name)) + 
  geom_col(width = 0.7) +
  scale_y_continuous(breaks=seq(0, 1, 0.1)) +
  coord_flip()
```

A serie 'The Walking Dead' mostra ser a mais irregular por ter um desvio padrao(que eh uma medida de dispers?o em torno da media populacional) eh maior que as outras series, com uma diferenca relativamente alta, dado que a diferenca no desvio padrao entre as outras eh cerca de no maximo 0.1 e dela para comparada a qualquer outra eh maior que 0.15.

Teste de distancia interquartil(IQR):
```{r}
  series %>%
    summarise(UserRating = IQR(UserRating)) %>%
    ggplot(aes(x = reorder(series_name, UserRating), y = UserRating, fill = series_name)) + 
    geom_col(width = 0.7) +
    scale_y_continuous(breaks=seq(0, 1, 0.1)) +
    coord_flip()
```
'The Walking Dead' tambem alcanca o maior IQR(que eh a diferen?a entre o quartil superior, que ? o valor onde 75% das observacoes estao abaixo dele e o quartil inferior onde 25% das observacoes estao abaixo, determina a intervalo interquartil.), tendo um IQR que ? quase duas vezes a de '13 Reasons Why', que ? a com menor IQR.

RESPOSTA:
  Dadas as observacoes se conclui que 'The Walking Dead' tem episodios de qualidade mais irregular segundo o IMDB dentre as quatro series escolhidas.
  A diferenca pode ser considerada alta dado que ela desponta com os maiores valores tanto no teste do desvio padrao quanto no do IQR.
