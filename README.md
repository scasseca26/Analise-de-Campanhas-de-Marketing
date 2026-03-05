# Analise-de-Campanhas-de-Marketing
![Power BI](https://img.shields.io/badge/Ferramenta-Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Concluído-brightgreen)

Projecto de análise de dados de campanhas de marketing com Power BI para identificar segmentos de clientes, padrões de compra e insights estratégicos.
---

## Problema do Negócio

Uma empresa necessitava de compreender melhor o perfil dos seus clientes, o comportamento de compra, a eficácia das suas campanhas de marketing e o desempenho dos seus pontos de venda entre 2019 e 2023, com o objectivo de tomar decisões mais informadas sobre onde e como investir em futuras campanhas.

O Gestor levantou questões organizadas em quatro visões analíticas:

**Visão Cliente**
1. Quantos clientes temos?
2. Qual é a média salarial anual dos clientes?
3. Em que meio os clientes mais compraram (Site, Loja, Catálogo)?
4. Qual é o total de clientes por nível de escolaridade e por estado civil?

**Visão Comportamento**
1. Qual foi o total gasto por salário anual?
2. Qual foi o total gasto por nível de escolaridade e por estado civil?
3. Quem tende mais a gastar — clientes com ou sem filhos e adolescentes em casa?

**Visão das Campanhas**
1. Qual foi a efectividade das campanhas em relação ao número de filhos em casa?
2. Qual é a percentagem das pessoas que compraram e das que não compraram?
3. Qual foi o total de visitas ao website e qual a sua relevância?
4. Qual foi a média salarial anual dos que compraram vs. os que não compraram?

**Visão dos Pontos de Venda**
1. Qual foi o total gasto em diferentes categorias por País?
2. Qual foi o total gasto por ano e por País?

---

## Contexto

A base de dados utilizada contém informações de 1.999 clientes de uma empresa com presença em múltiplos países, cobrindo o período de 2019 a 2023. Os dados incluem o perfil demográfico dos clientes, os seus hábitos de consumo por categoria de produto, a adesão às campanhas de marketing e os canais de compra utilizados. A análise foi organizada em quatro visões distintas para facilitar a leitura e a tomada de decisão.

> **Fonte dos dados:** [Data Science Academy](https://www.datascienceacademy.com.br)

---

## Premissas da Análise

- Os dados foram tratados e analisados no **Power BI**.
- Antes de carregar os dados para análise, foram realizadas as seguintes transformações no **Power Query**:

| Transformação | Descrição |
|---------------|-----------|
| **Coluna "Comprou"** | Os valores originais 0 e 1 foram substituídos por *Não* e *Sim*, respectivamente, para maior clareza na leitura dos relatórios |
| **Remoção de Outlier** | Removidos os registos com valores absurdos na coluna de salário (valor *666666*), que distorciam os resultados da análise |

- Foi criada uma **medida DAX** para calcular o total gasto agregando todas as categorias de produtos:

```dax
Total Gasto =
SUMX(
    dados_marketing,
    dados_marketing[Gasto com Alimentos] +
    dados_marketing[Gasto com Brinquedos] +
    dados_marketing[Gasto com Eletronicos] +
    dados_marketing[Gasto com Moveis] +
    dados_marketing[Gasto com Utilidades] +
    dados_marketing[Gasto com Vestuario]
)
```

- A análise cobre todos os países presentes na base de dados e o período de **2019 a 2023**.
- A coluna `Comprou` é a variável central das análises de campanha, indicando se o cliente efectuou alguma compra após contacto.

---

## Estratégia da Solução

### Passo 1 — Resumo do Contexto em Pergunta Aberta
> *Qual é o perfil dos clientes desta empresa, como se comportam nas campanhas de marketing e onde geram mais valor?*

### Passo 2 — Transformação em Perguntas Fechadas
> - Quem são os clientes em termos de perfil demográfico e financeiro?
> - Que características definem os clientes que mais gastam?
> - As campanhas de marketing estão a converter os clientes certos?
> - Quais países e categorias de produto geram mais receita?

### Passo 3 — Definição da Coluna Facto
A coluna facto principal é **Total Gasto**, calculada via medida DAX, pois agrega o valor consumido por cliente em todas as categorias e é a base para medir o desempenho financeiro da empresa.

### Passo 4 — Identificação das Dimensões

| Dimensão | Coluna |
|----------|--------|
| Perfil Demográfico | `Escolaridade`, `Estado Civil`, `Ano Nascimento` |
| Perfil Financeiro | `Salario Anual` |
| Comportamento de Compra | `Numero de Compras na Loja`, `Numero de Compras na Web`, `Numero de Compras via Catalogo` |
| Composição Familiar | `Filhos em Casa`, `Adolescentes em Casa` |
| Campanhas | `Compra na Campanha 1 a 5`, `Comprou` |
| Geografia | `País` |
| Tempo | `Data Cadastro`, `Mes` |
| Categorias de Gasto | `Gasto com Alimentos`, `Gasto com Brinquedos`, `Gasto com Eletronicos`, `Gasto com Moveis`, `Gasto com Utilidades`, `Gasto com Vestuario` |

### Passo 5 — Hipóteses Analíticas

- H1: A loja física é o canal de compra preferido pela maioria dos clientes.
- H2: Clientes com maior salário anual tendem a gastar mais.
- H3: Clientes solteiros e com ensino superior são os maiores consumidores.
- H4: Clientes sem filhos em casa aderem mais às campanhas e gastam mais.
- H5: A taxa de conversão das campanhas é baixa em relação ao total de clientes contactados.
- H6: Os Estados Unidos são o país com maior volume de gastos em todas as categorias.

### Passo 6 — Critérios de Priorização

As hipóteses foram priorizadas com base em dois critérios:

- **Impacto no Negócio** — quanto o insight pode influenciar directamente a estratégia de marketing e vendas.
- **Relevância Estratégica** — quanto a informação orienta decisões sobre segmentação de clientes e alocação de investimento.

### Passo 7 — Priorização das Hipóteses Analíticas

| Prioridade | Hipótese | Justificativa |
|------------|----------|---------------|
| Alta | H5 — Taxa de conversão das campanhas | Mede directamente a eficácia do investimento em marketing |
| Alta | H3 — Perfil do cliente que mais gasta | Orienta a segmentação para futuras campanhas |
| Alta | H6 — Desempenho por país e categoria | Informa decisões de expansão e foco de produto |
| Média | H4 — Impacto dos filhos no comportamento de compra | Segmenta melhor o público-alvo das campanhas |
| Média | H2 — Salário anual vs. total gasto | Valida o critério de segmentação por poder de compra |
| Baixa | H1 — Canal de compra preferido | Confirma onde concentrar esforços de distribuição |

---

## Insights da Análise

### Visão Cliente

A empresa possui **1.999 clientes** com uma **média salarial anual de 51,98K**. O canal de compra preferido é a **Loja Física**, sendo o meio onde os clientes mais realizam as suas compras em comparação com o Website e o Catálogo.

Em relação ao perfil demográfico:

| Escolaridade | Nº de Clientes |
|-------------|---------------|
| Superior | 999 |
| Doutorado | 437 |
| Mestrado | 337 |
| Segundo Grau | 177 |
| Primeiro Grau | 49 |

| Estado Civil | Nº de Clientes |
|-------------|---------------|
| Solteiro | 1.197 |
| Casado | 522 |
| Divorciado | 280 |

### Visão Comportamento

Os dados confirmam que **clientes com maior salário anual são os que mais gastam**, evidenciando uma correlação directa entre poder de compra e volume de consumo. Ao cruzar escolaridade e estado civil, os **Solteiros com Ensino Superior** destacam-se como o grupo que mais gasta, o que é consistente com o facto de serem o maior segmento da base de clientes. Adicionalmente, os clientes **sem filhos em casa e sem adolescentes** tendem a gastar significativamente mais, reforçando que a ausência de dependentes liberta maior capacidade financeira para o consumo.

### Visão das Campanhas

Os dados revelam que a **taxa de conversão das campanhas é baixa**: apenas **16,01%** dos clientes efectuaram alguma compra, enquanto **83,99%** não comprou. Dos 1.144 clientes sem filhos em casa — o grupo mais representativo entre os que aderiram às campanhas — apenas **208 converteram em compra**, enquanto **936 não compraram**.

O website registou um total de **10.649 visitas**, das quais apenas **1.703 geraram uma compra**, o que reforça a necessidade de optimizar a experiência de compra online para melhorar a conversão.

Um dado relevante: a **média salarial dos que compraram (59.489)** é superior à dos **que não compraram (50.538)**, o que sugere que as campanhas estão a atrair um perfil de maior poder de compra, mas ainda com baixa conversão geral.

### Visão dos Pontos de Venda

Os **Estados Unidos** foram o país com maior volume de gastos em todas as categorias analisadas, com os **Electrónicos** a destacarem-se como a categoria mais consumida tanto nos EUA como nos restantes países. Esta tendência manteve-se ao longo de todos os anos analisados, com os Estados Unidos a liderar os gastos na grande maioria dos períodos entre 2019 e 2023.

---

## Resultados

<table>
  <tr>
    <td align="center">
      <img width="1457" height="818" alt="Dasboard1" src="https://github.com/user-attachments/assets/1db12b3e-e97e-4a08-9c5c-fe5f6a867d60" /><br>
      <em>*Figura 1: Dashboard da Visão Cliente.*</em>
    </td>
    <td align="center">
      <img width="1470" height="819" alt="Dashboard2" src="https://github.com/user-attachments/assets/c961c40e-c709-4e82-8f85-6e77316dc29e" /><br>
      <em>*Figura 2: Dashboard da Visão Comportamento.*</em>
    </td>
  </tr>
  <tr>
    <td align="center">
    <img width="1460" height="815" alt="Dashboard3" src="https://github.com/user-attachments/assets/4802ba55-7a08-4f4b-8c45-b892d81505e6" /><br>
      <em>*Figura 3: Dashboard da Visão das Campanhas.*</em>
    </td>
    <td align="center">
      <img width="1459" height="817" alt="Dashboard4" src="https://github.com/user-attachments/assets/9af5b1b3-1590-4cdb-83d3-60c5eb7e5e27" /><br>
      <em>*Figura 4: Dashboard da Visão dos Pontos de Vendas.*</em>
    </td>
  </tr>
</table>

Com base na análise, é possível concluir que:

- A empresa deve focar as suas campanhas no perfil Solteiro com Ensino Superior e sem filhos, pois é o segmento com maior propensão e capacidade de gasto.
- A **taxa de conversão de 16%** é reduzida e deve ser melhorada — recomenda-se investir na personalização das campanhas para os segmentos com maior poder de compra.
- A experiência de compra no Website deve ser optimizada, dado que a grande maioria das visitas não resulta em compra.
- Os **Estados Unidos e a categoria de Electrónicos** devem continuar a receber prioridade no investimento em marketing, por serem os maiores geradores de receita.
- Clientes com **maior salário anual convertem mais**, pelo que as campanhas devem ser segmentadas com base no poder de compra para maximizar o retorno do investimento.

---

## Estrutura do Projecto

```
 analise-de-campanhas-de-marketing
│
├──  dados
│   └── dados_marketing.csv              # Base de dados original
├──  analise
│   └── Analise Marketing.pbix           # Ficheiro Power BI com análises e dashboard
└──  README.md                           # Documentação do projecto
```

---

## Ferramentas Utilizadas

- **Power BI** — Para tratamento de dados via Power Query, criação de medidas DAX, visualizações interactivas e Dashboard

---

## Autor

**Santiago Casseca**  
[LinkedIn](https://linkedin.com) 
