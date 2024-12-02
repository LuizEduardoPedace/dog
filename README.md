# Introdução 🐕

Mais do que nunca, o melhor amigo do homem faz juz ao seu nome. Em grandes capitais como São Paulo, é difícil observar algum momento do dia em que não há pessoas passando o tempo com seus cães, o que por diversas vezes, é comparado ao relacionamento de um filho ou parente próximo.

No entanto, ainda vemos muitos cães nas ruas e as taxas de abandono de animais continuam elevadas. Escolher um *pet* deve estar atrelado às suas escolhas pessoas e ao seu estilo de vida, fatores como dinheiro, traço de temperamento do animal e tamanho devem ser levados em conta na hora de escolher o seu companheiro.

Por esse motivo, a partir de um *dataset* criei um ***dashboard*** com filtros relevantes para te ajudar a escolher qual a **raça de cão** 🐶 mais compatível com seu **perfil**! Além disso, realizei uma análise aprofundada utilizando algumas técnicas de **estatística** para encontrar *insights* interessantes no que diz respeito ao preço da raça e ao traço de temperamento.


# Ferramentas Utilizadas 🛠️

- **Excel:** O software utilizado para importação do *dataset* e realização do EDA (*Exploratory Data Analysis*);

- **Power Query:** O software utilizado para a extração, transformação e carregamentos dos dados;

- **PowerBI:** O software utilizado para construir um *dashboard* com filtros interativos;

- **Power Pivot:** O ambiente utilizado para criar relações entre as tabelas e medidas úteis;

- **Visual Studio Code:** O ambiente interativo utilizado para criar os códigos com facilidade e rapidez;

- **Python:** A linguagem de programação utilizada para realizar a análise a partir da manipulação de dados, construção de visualizações e usufruo de técnicas estatísticas. Foram utilizadas bibliotecas como: numpy, pandas, seaborn, matplotlib, scipy;

- **Jupyter Notebook:** Formato de arquivo utilizado com a linguagem de programação python para organização do código;

- **Git & GitHub:** O software utilizado para monitorar as modificações feitas no projeto no decorrer do desenvolvimento, e publicá-lo em um repositório de fácil acesso aos dados utilizados, os códigos construídos e ao relatório final.


# *Dataset* 🗂️

O *dataset* escolhido está no repositório como [dog_breed_characteristics.csv](/dog_breed_characteristics.csv). Ele contém as seguintes colunas:

- 🐶 **Nome da Raça**

- 💲 **Preço médio**

- 🐾 **Grupo**

- ⚖️ **Peso**

- ⚠️ **Nível de vigilância**

- 👑 **Popularidade**

- 🎭 **Temperamento**

As demais colunas contidas no *dataset* serão removidas no processo de *ETL (Extract, Transform and Loading)* dos dados.


# Extração, Transformação e Carregamento dos Dados ⛏️

O *dataset* original contém muitos erros. Há colunas cuja maioria dos dados é nula, erros de digitação, linhas duplicadas, entre outras coisas. É de suma importância explorar os dados inicialmente para retirarmos informações incoerentes com o todo. Para esta etapa, utilizei o **Excel**, **Power Query** e o **Python**.

## 📤 **Extração**

O *dataset* original ```dog_breed_characteristics.csv``` foi carregado no Excel, onde pude criar uma tabela para incorporar ao Power Query e realizar as modificações necessárias. Foram criadas duas ***Queries***:

- 🐶 **dog_characteristics**

![](/assets/ETL/dog_characteristics_extract.PNG)

- 🍀 **dog_trait** 

![](/assets/ETL/dog_trait_extract.PNG)

A primeira tabela contém as características gerais de cada raça como preço, peso, grupo etc. A segunda tabela possui a lista de traços de temperamento para cara raça de cão.

## 🔄 Transformação

Foi necessários corrigir os erros de dados citados em cada uma das tabelas criadas. 

- 🐶 **dog_characteristics**

Removi colunas inúteis, alterei o tipo de dado, corrigi dados duplicados, filtrei apenas as informações relevantes, renomiei e reordenei colunas, entre outras coisas.

![](/assets/ETL/dog_characteristics_transform.PNG)

- 🍀 **dog_trait** 

Foi necessário separar cada um dos traços de temperamento para cada uma das raças de modo que fosse possível corresponder esta *querie* com a primeira.

![](/assets/ETL/dog_trait_transform.PNG)

O documento de Excel com as *queries* e transformações realizadas pode ser encontrado em [dog_breed_characteristics.xlsx](/dog_breed_characteristics.xlsx).

## 🔗 Carregamento

De posse dos dados corrigidos, criei um arquivo *.csv* para cada *querie* (```dog_characteristics.csv``` e ```dog_trait.csv```), a fim de incporporar ao *jupyter notebook* para realizar a análise subsequente.

```python
import pandas as pd

# dog_characteristics
df_1 = pd.read_csv('dog_characteristics.csv', delimiter=';')

# dog_trait
df_2 = pd.read_csv('dog_trait.csv', delimiter=';')
```


# Dashboard 📶

Antes da análise, confira o *dashboard* que eu criei:

📌 [Dashboard](https://app.powerbi.com/view?r=eyJrIjoiZTdjM2U1OTUtOWUyOC00MjY0LWEzNzktN2Q3ZWZmZGNmMTFmIiwidCI6IjdlOTNlMjg2LWIyOWEtNDQ1NC1hNDFhLWU4NDE5ZWM5ZGViNSJ9)

![](/assets/dashboard/dashboard_gif.gif)

Você poderá escolher diversos filtros e ele retornará a raça de cão que mais atende ao seu perfil.
Você encontrará os seguintes **filtros**:

- 💲 **Preço médio**

- 🐾 **Grupo**

- ⚖️ **Peso**

- ⚠️ **Nível de vigilância**

- 🎭 **Temperamento**

Para construí-lo, importei o Power Query desenvolvido anteriormente no **Power BI**.  Foi necessário a criação de uma nota tabela para filtrarmos as raças por grupo:

![](/assets/dashboard/dog_group_table.PNG)

Posteriormente, como inserimos filtros a partir de três tabelas diferentes, foi necessário utilizar o **Power Pivot** para criar relações entre as tabelas a partir do **nome das raças**:

![](/assets/dashboard/power_pivot_relationships.PNG)

Com isso, estamos prontos para a criação do *dashboard*. Ao lado esquerdo encontra-se uma lista das raças de cães que **atendem aos requisitos** e seu respectivo **preço**:

![](/assets/dashboard/list_of_breeds.PNG)

No canto superior encontra-se a raça de cão **mais cara** que atende aos requisitos e seu respectivo preço. Necessitou-se a criação de uma medida para encontrarmos o nome:

```dax
top_breed = MAXX(TOPN(1,dog_characteristics,dog_characteristics[Price],DESC),dog_characteristics[BreedName])
```

![](/assets/dashboard/expensive_breed.PNG)


# Análise 📊

A análise feita está em ```main.ipynb```, nele está contido um *jupyter notebook* detalhado com anotações, os códigos utilizados, visualizações do relatório e equações.

Vamos começar a análise estudando como o preço médio está distribuído no *dataset*. Para isso, utilizaremos estatística descritiva:

```python
df_1['Price'].describe().round(2)
```

| Estatística   | Valor      |
|---------------|------------|
| Mediana       | $ 900.00   |
| Média         | $ 1045.04  |
| Desvio Padrão | $ 518.73   |
| Mínimo        | $ 350.00   |
| Máximo        | $ 3000.00  |
| 1° quartil    | $ 700.00   |
| 2° quartil    | $ 900.00   |
| 3° quartil    | $ 1350.00  |
| Variância     | $ 269083.19|

*Tabela com algumas estatísticas descritivas para os preços das raças de cães.*

Note como a faixa de preço dos cães pode variar bastante, chegando a preços de **$3000.00**! A média dos preços é elevada considerando-se o salário mínimo e a variância dos dados é descomunal. Vamos entender depois o motivo da variância ser tãoi elevada com testes equivalentes ao **ANOVA**.

Podemos usufruir do boxplot para obter informação visual de como os dados estão distribuídos:

![](/assets/analysis/boxplot_price_outliers.png)

*Boxplot dos preços para as raças de cães.*

Note a presença de ***outliers***, o que explica o valor da média ser maior do que a mediana. Além do mais, **50%** dos dados estão entre (**\$700.00**, **\$1350.00**), a partir daí os preços começam a dar grandes saltos como é possível ver pela largura da parte de cima do boxplot.

Quais raças são responsáveis pelos *outliers*? Sabemos que o piso superior e inferior são calculados da seguinte maneira:

$$\text{Lower Outlier} = Q_1 - (1.5 \cdot \text{IQR})$$
$$\text{Higher Outlier} = Q_3 + (1.5 \cdot \text{IQR})$$
$$\text{IQR} = Q_3 - Q_1$$

Com isso, podemos calculá-los diretamente no python:

```python
Q1 = df_1['Price'].quantile(.25) # Primeiro Quartil
Q3 = df_1['Price'].quantile(.75) # Terceiro Quartil
IQR = Q3 - Q1 #Interquartil
low_out = Q1 - (1.5 * IQR) # Piso Inferior
hig_out = Q3 + (1.5 * IQR) # Piso Superior

# Tabela de Outliers
outliers = df_1[df_1['Price'] > hig_out].sort_values('Price', ascending=1)
``` 

| Nome da Raça               | Preço     |
|----------------------------|-----------|
| French Bulldog             | $ 3000.00 |
| Kai Dog                    | $ 3000.00 |
| Tibetan Mastiff            | $ 3000.00 |
| Pumi                       | $ 2750.00 |
| Coton de Tulear            | $ 2700.00 |
| Portuguese Water Dog       | $ 2650.00 |
| Sussex Spaniel             | $ 2500.00 |

*Tabela com os outliers e seus respectivos preços.*

Vemos que raças mundialmente conhecidas como **Buldogue Francês** e **Mastim Tibetano** lideram o preço das raças de cães. Podemos nos perguntar qual o traço de temperamento mais comum dentre esses *outliers*.

```python
outliers_trait = df_2[df_2['BreedName'].isin(outliers['BreedName'])].groupby('Trait').count().sort_values('BreedName', ascending=0)[0:10]
```

![](/assets/analysis/hbar_outliers_count_trait.png)

*Gráfico de barras da contagem de traços para os outliers.*

Traços como **inteligente** e **vivaz** são os mais comuns dentre as raças de cães mais caras. Mais adiante vamos obter o preço médio para cada traço de temperamento e averiguaremos se estes mesmos traços estão no Top 10.

Vamos agora retirar os *outliers* do *dataset* para analisarmos tendências e entendermos os dados com mais afinco.

```python
df_out = df_1[df_1['Price'].isna() == False].drop(index=outliers.index)
df_out['Price'].describe().round(2)
```

| Estatística   | Valor       |
|---------------|-------------|
| Mediana       | $ 900.00    |
| Média         | $ 992.77    |
| Desvio Padrão | $ 425.69    |
| Mínimo        | $ 350.00    |
| Máximo        | $ 2250.00   |
| 1° quartil    | $ 700.00    |
| 2° quartil    | $ 900.00    |
| 3° quartil    | $ 1275.00   |
| Variância     | $ 181208.13 |

*Tabela com algumas estatísticas descritivas para os preços das raças de cães do dataset sem os outliers.*

A média diminui e se torna mais próxima da mediana como era de se esperar. O valor máximo agora é de **\$2250.00**, note que a variância continua bastante elevada, iremos tentar entender o motivo disso.

![](/assets/analysis/histogram_price.png)

*Histograma do preço das raças de cães sem os outliers.*

Existe uma predominância dos preços entre **\$700.00** e **\$1100.00** como é possível ver pelo pico do histograma. Note também que há um acúmulo de valores entre **\$350.00** e **\$600.00**. Podemos nos pergunta se os dados acima são bem descritos por uma distribuição normal, para isso, podemos efetuar uma **análise visual** bem como um teste de estatística.

![](/assets/analysis/kde_price.png)

*Kernel Density Estimation para o preço das raças de cães.*

Visualmente percebemos que não se aproxima de uma distribuição normal. Vamos utilizar o ***Quantile-Quantile plot***:

```python
import scipy.stats as stats

stats.probplot(df_out['Price'], dist='norm', plot=plt)
```

![](/assets/analysis/q_q_plot_price.png)

*Quantile-Quantile plot para o preço das raças de cães.*

Pela visualização *quartil-quartil* os dados definitivamente não são bem descritos por uma distribuição normal. Porém, podemos ainda utilizar o **Teste de Anderson-Darling**:

```python
stats.anderson(df_out['Price'])
```

| Estatística                 | Valor |
|-----------------------------|-------|
| Anderson-Darling            | 4.02  |
| Valor crítico ($\alpha=.05$)| 0.774 |

*Tabela com o Teste de Anderson-Darling e seus respectivos resultados.*

Para um **valor de significância** de $\alpha=.05$, nós definitivamente podemos **descartar a hipótese nula** e portanto os dados **não são bem descritos por uma distribuição normal**.

Agora que testamos a normalidade, vamos utilizar testes **não-paramétricos** para **estudar a variância** e realizar **testes de hipótese**.

## Análise por Grupo 🐾

Como vimos, a variância dos dados é elevada, vamos separar as raças de cães por **Grupo** e estudar se eles estão de alguma forma atrelados á essa dispersão.

```python
df_group = df_out[['BreedName','Group1','Group2','Price']].copy()
df_group['Group'] = df_out['Group1'] + ',' + df_out['Group2']
df_group['Group'] = df_group['Group'].str.split(',')
df_group.drop(columns=['Group1','Group2'], inplace=True)
df_group = df_group.explode('Group', ignore_index=True)
```

![](/assets/analysis/boxplot_group.png)

*Boxplot do preço das raças por Grupo.*

Vemos que grupos como **Guardião** e **Trabalhador** possuem uma média de preço maior do que a média, enquanto **Cão Farejador** está bem abaixo. Visualmente podemos explicar a dispersão dos dados devido à variação de preço entre os grupos, no entanto, podemos efetuar um teste estatístico para quantizar essa variação. Porquanto, utilizaremos o **Teste de Kruskal-Wallis** para esta análise.

```python
values = df_group[df_group['Group'].isna() == False]['Group'].unique()
data1, data2, data3, data4, data5, data6, data7, data8, data9, data10, data11, data12, data13 = [df_out[df_group['Group'] == g]['Price'] for g in values]
stats.kruskal(data1, data2, data3, data4, data5, data6, data7, data8, data9, data10, data11, data12, data13)
```

| Estatística           | Valor |
|-----------------------|-------|
| Kruskal-Wallis        | 18.00 |
| valor-p               | 0.11  |

*Tabela com o Teste de Kruskal-Wallis e seus respectivos resultados.*

Para um **valor de significância** de $\alpha=.05$ nós **não** podemos descartar a **hipótese nula**. Pela visualização, o grupo Cão Farejador se destaca dentre os demais devido ao seu preço médio ser bem inferior à media total. Vamos estudar esse grupo com mais afinco.

```python
Scenthound = df_group[df_group['Group'] == 'Scenthound']
```

Iniciaremos fazendo uma análise descritiva.

| Estatística    | Valor       |
|----------------|-------------|
| Mediana        | $ 500.00    |
| Média          | $ 663.00    |
| Desvio Padrão  | $ 412.00    |
| Mínimo         | $ 350.00    |
| Máximo         | $ 1900.00   |
| 1° quartil     | $ 400.00    |
| 2° quartil     | $ 500.00    |
| 3° quartil     | $ 675.00    |
| Variância      | $ 169956.14 |

*Tabela com algumas estatísticas descritivas para os preços do grupo Scenthound.*

Percebemos que os valores para a média e mediana são bem inferiores com relação ao *dataset* original. A média está acima da mediana, o que pode demonstrar a existência de *outliers* e a variância permanece elevada dentro do próprio grupo.

![](/assets/analysis/boxplot_scenthound.png)

*Boxplot da faixa de preço do grupo Scenthound.*

Como analisado anteriormente, há a existência de *outliers*, enquanto que a média está à parte da maior parte dos dados, revelando que algumas raças estão com um preço muito acima da maioria. Por esse motivo, vamos encontrar os *outliers* e separá-los dos demais dados do grupo.

| Nome da Raça                    | Preço   |
|---------------------------------|---------|
| Irish Wolfhound                 | $ 1900.0|
| Petit Basset Griffon Vendeen    | $ 1400.0|
| English Coonhound               | $ 1100.0|

*Tabela com os outliers do grupo Scenthound e seus respectivos preços.*

É possível ver qeu os *outliers* estão muito acima da média do grupo. Enquanto 50% dos valores estão entre **\$400.00** e **\$675.00**, eles estão com preços maiores do que o dobro. Retirando-se os *outliers*, os dados **não** são bem descritos por uma **distribuição normal**, os testes feitos foram os mesmos feitos anteriormente e se encontram no [*jupyter notebook*](/main.ipynb).

Deste modo, vamos utilizar um **teste de hipótese** não-paramétrico para averiguar se com esses dados podemos inferir que a média do grupo é inferior à media total. Utilizaremos o **Teste de Wilcoxon**:

```python
stats.wilcoxon(Scenthound_data['Price'] - df_out['Price'].mean())
```

| Estatística           | Valor   |
|-----------------------|---------|
| Wilcoxon              | 1.00    |
| valor-p               | 6.10e-05|

*Tabela com o Teste de Wilcoxon e seus respectivos resultados.*

Indubitavelmente podemos **descartar** a **hipótese nula** e inferir que a média de preço do grupo Scenthound é **inferior à media total**.

## Análise por Nível de Vigilância ⚠️

Vamos agora separar os grupos por **nível de vigilância** das raças. Neste *dataset* a vigilância é uma **variável categórica** de 1 a 6.

![](/assets/analysis/boxplot_watchdog.png)

*Boxplot do preço das raças por Nível de Vigilância.*

Pela visualização, a média de preço não varia tanto como na separação por Grupo. Vamos efetuar o **Teste de Kruskal-Wallis** para analisar se podemos inferir que essa separação está atrelada à dispersão dos dados.:

| Estatística           | Valor |
|-----------------------|-------|
| Kruskal-Wallis        | 12.82 |
| valor-p               | 0.02  |

*Tabela com o Teste de Kruskal-Wallis e seus respectivos resultados.*

Para um **valor de significância** de $\alpha=.05$ podemos **descartar** a **hipótese nula** e inferir que as médias de cada grupo diferem. Notemos ainda que pode existir uma certa **correlação** entre a média de preços e o nível de vigilância:

![](/assets/analysis/scatter_price_x_watchdog.png)

*Gráfico do Preço Médio em função no Nível de Vigilância.*

Ao considerarmos a média de preço em função do nível de vigilância e testar a normalidade dos dados, podemos inferir que eles são **bem descritos** por uma **distribuição normal**. Os testes feitos são os mesmos e estão no [*jupyter notebook*](/main.ipynb). Com isso, podemos utilizar a **Correlação de Pearson** (ao considerarmos o nível de vigilância uma **variável métrica**):

```python
# Dados
watchdog = df_out.groupby('Watchdog')['Price'].mean().index.tolist()
watchdog_avg = df_out.groupby('Watchdog')['Price'].mean().values.tolist()
# Teste
stats.pearsonr(watchdog, watchdog_avg)
```

| Estatística           | Valor |
|-----------------------|-------|
| Pearson r             | 0.77  |
| valor-p               | 0.07  |

*Tabela com a Correlação de Pearson e seus respectivos resultados.*

Por mais que a correlação $r$ de Pearson tenha sido elevada, devido à **falta de dados**, **não** podemos descartar a **hipótese nula** e inferir que existe uma correlação entre as variáveis.

No *dataset* possuímos as variáveis métricas ⚖️ **Peso** e 👑 **Popularidade**, vamos testar a correlação entre estas variáveis e o preço:

![](/assets/analysis/heatmap_corr.png)

*Mapa de calor das correlações entre Preço, Peso e Popularidade .*

Pelo mapa de calor acima vemos que não há nenhuma correlação significante entre as três variáveis.


## Traço de Temperamento 🎭

Por fim, vamos obter alguns *insights* a partir da análise dos traços de temperamento. Primeiramente, vamos entender como estes estão distribuídos:

![](/assets/analysis/pie_top10_traits.png)

*Gráfico de pizza para os 10 traços mais comuns.*

Vemos que o traço **inteligente** é o mais comum, o que demonstra não estar atrelado necessariamente às raças caninas mais caras como vimos anteriormente. Vamos averiguar isto mais a fundo obtendo o preço médio atrelado a cada traço:

```python
trait_price = {}
for t in df_2['Trait'].unique().tolist():
    trait_price[t] = df_1[df_1['Temperament'].str.contains(t, case=False)]['Price'].mean()
trait_price = pd.DataFrame.from_dict(trait_price, orient='index')
trait_price = trait_price.rename(columns={0: 'Price'})
trait_price.index.name = 'Trait'
trait_price.reset_index(inplace=True)

top_traits = trait_price.sort_values('Price',ascending=0)[0:10].round(2)
```

![](/assets/analysis/hbar_top10_trait_price.png)
*Gráfico de barras para os 10 traços de temperamento mais caros*

Traços como **Briguento** e **Impetuoso** estão entre os mais caros custando acima de **\$2500.00**! Os traços inteligente e vivaz vistos anteriormente nos outliers não estão presentes.

Vamos agora visualizar o boxplot para os 15 traços mais comuns e testar se isso explica a dispersão de variância encontrada nos dados.

![](/assets/analysis/boxplot_trait.png)

*Boxplot do preço em função dos 15 traços mais comuns.*

Vemos que a média de preço dos traços não dista de forma sifnificante, ao utilizarmos o Teste de Kruskal-Wallis obtivemos que:

| Estatística           | Valor |
|-----------------------|-------|
| Kruskal-Wallis        | 13.47 |
| valor-p               | 0.49  |

*Tabela com o Teste de Kruskal-Wallis e seus respectivos resultados.*

Com isso, **não** podemos descartar a **hipótese nula** e isso não explica a dispersão encontrada.

# Conclusão 💡

Primeiramente, realizamos uma **análise descritiva** dos preços e estudamos seus ***outliers***, procuramos obter alguma relação entre seus preços elevados e traços comportamentais. Vimos que **inteligente** e **vivaz** são os mais comuns entre as raças mais caras. 

Com o usufruo de boxplots e a partir do valor da variância, averiguamos que faixa de preço pode variar bastante dentro do *dataset*, então procuramos entender de onde se origina tal dispersão, para isso, foi utilizado a contraparte **não-paramétrica** do ANOVA (**Kruskal-Wallis**). Separamos os dados por 🐾 **Grupo**, ⚠️ **Nível de Vigilância** e 🎭 **Traço de Temperamento**. Obtivemos que o Nível de Vigilância e Grupo explicam melhor a origem da dispersão. Encontramos também uma **correlação** forte entre preço e vigilância, mas devido à falta de dados não podemos inferir a partir de um **valor de significância** de $\alpha=.05$.

O grupo **Cão Farejador** possui uma média de preço bem abaixo da média, com exceção de poucas raças, a partir do **teste de hipótese não-paramétrico Wilcoxon** obtivemos um valor-p indubitavelmente menor que o valor de significância. Demonstrando que o grupo de fato possui uma média de preço inferior aos demais.

Por fim, estudamos a distribuição de traços de temperamento, obtivemos que **Briguento** e **Impetuoso** possuem os maiores preços médios, com valores acima **\$2500.00**!