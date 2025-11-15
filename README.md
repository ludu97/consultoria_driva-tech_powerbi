# consultoria_driva-tech_powerbi (Em Construção)
Consultoria realizada para uma empresa fictícia varejista de moda com 5 lojas físicas em Curitiba.<br>
<br>
**Objetivo**: Encontrar padrões e tendências e sugerir estratégias para otimizar a distribuição de produtos e promoções nos PDV's, visando aumento de vendas e melhoria na satisfação do cliente.<br>
O cliente forneceu dados das vendas das lojas e dados demográficos dos consumidores.

# Desafio
Um varejista de moda, cliente fictício da DrivaTech, busca entender melhor o desempenho de suas lojas físicas para otimizar estratégias de distribuição de produtos e promoções, visando aumento das vendas e melhoria na satisfação do cliente. O cliente forneceu dados das vendas das lojas e dados demográficos dos consumidores.<br>
<br>

## Esquema de Dados das Tabelas Originais:
<img width="1074" height="431" alt="image" src="https://github.com/user-attachments/assets/c5a95d55-59ca-4095-9d50-0f7f7ccb9f46" />
<br>

**Objetivo**:
1. Examinar os dados para identificar padrões de vendas e diferenças regionais;<br>
2. Aplicar técnicas básicas de segmentação para identificar diferentes perfis de consumidores. Analisar suas preferências de compra para entender quais produtos ou promoções são mais eficazes para cada segmento;<br>
3. Elaborar recomendações práticas sobre como ajustar a distribuição de produtos e as estratégias de marketing para melhor atender aos diferentes segmentos de clientes, baseando-se nos insights extraídos das análises anteriores.<br>

# Metodologia

### 1. Origem dos Dados: 
Fictícios. Gerados de forma randômica, porém seguindo modelo de consultoria para aplicação de conceitos e estudo.<br>

### 2. Transformação e Carregamento: 
Carregamento das planilhas normalizadas em Power BI e Transformação em Power Query para limpeza, criação de novas colunas, medidas e criação da tabela calendário. Abaixo compartilho algumas das fórmulas DAX que utilizei e o seu objetivo:<br>

### 3. Algumas Fórmulas utilizadas:

#### A. Criação da Tabela Calendário:  
Esta fórmula DAX para a criação da tabela calendário é uma melhor opção do que o Auto Date/Time do Power BI, pois prioriza a performance e a capacidade de análise avançada (Time Intelligence) no projeto, evitando o inchaço (model bloat) que a fórmula automática Auto Date/Time geraria ao criar diversas tabelas ocultas de data para cada coluna de data no projeto.
```
DIM_Calendario =
ADDCOLUMNS (
    CALENDAR (MIN(FATO_Vendas[DATA_VENDA]), MAX(FATO_Vendas[DATA_VENDA])),
    "Ano", YEAR([Date]),
    "Mês", FORMAT([Date], "MMMM"),
    "Mês/Ano", FORMAT([Date], "YYYY-MM"),
    "Dia da Semana", FORMAT([Date], "dddd"),
    "Dia_da_Semana_Num", WEEKDAY([Date], 2) // 1=Segunda
)
```
**Objetivo na Análise**:<br>
O principal objetivo é permitir realizar Análises Temporais Avançadas (Time Intelligence) de forma correta e flexível, pois possibilita:<br>
- Agrupamento: Permite agrupar vendas por Mês, Dia da Semana, Trimestre, ou Dia Útil/Fim de Semana.<br>
- Comparação: É o motor para métricas complexas como "Vendas Mês Anterior" ou "Vendas Ano Passado", que exigem uma sequência de datas ininterrupta.<br>
- Filtro: Permite filtrar todas as suas métricas (Vendas, Ticket Médio, etc.) usando atributos de tempo (como "Mês", "Dia da Semana") em vez de usar apenas a data bruta.<br>
<br>

#### B. Agregação por Cliente (Proxy LTV)
Esta fórmula é uma Coluna Calculada na tabela DIM_Clientes e é a base para a segmentação de clientes por Lifetime Value (LTV). Ela Resolve o problema de agregar dados transacionais (muitas linhas na FATO) em uma tabela de dimensão (uma linha por cliente), usando a técnica de Context Transition (CALCULATE) e ignora/aplica filtros (FILTER(ALL())) para garantir que o gasto total de cada cliente seja preciso.
```
VALOR_GASTO_CLIENTE =
CALCULATE(
    [VALOR_VENDA_TOTAL],
    FILTER(
        ALL('FATO_Vendas'),
        'FATO_Vendas'[VENDAS.ID_CLIENTE] = 'DIM_Clientes'[ID_CLIENTE]
    )
)
```
**Objetivo na Análise**:<br>
Esta fórmula permite medir o valor monetário total que cada cliente contribuiu para o negócio, desde o início dos dados, sem interferências de filtros gerais.<br>
<br>

#### C. Benchmark Dinâmico (Média Geral Ajustável)
Esta métrica é essencial para criar a linha de referência nos gráficos de Faturamento por Filial e Ticket Médio. Ela calcula a média da rede, mas tem a inteligência de se ajustar a filtros de data ou cliente (tornando-a dinâmica).<br>
```
MEDIA_FATURAMENTO_GERAL_FILIAL_DINAMICA =
VAR FaturamentoGeral = CALCULATE([VALOR_VENDA_TOTAL], ALL(DIM_Filiais))
VAR NumFiliais = COUNTROWS(ALL(DIM_Filiais))
RETURN
    DIVIDE(FaturamentoGeral, NumFiliais)
```
**Objetivo na Análise**:<br>
Esta métrica permite avaliar o desempenho das filiais em relação à média de faturamento que cada filial deveria atingir no período e contexto filtrado (se o usuário filtrou por "Novembro", a média de novembro é calculada).<br>
<br>

# Modelagem de Dados: 
Star Schema em Power BI, a partir de 5 planilhas de Excel normalizadas<br>
<img width="1118" height="661" alt="image" src="https://github.com/user-attachments/assets/5120c036-1d5c-4067-ad7f-988b4e004dae" />
<br>
<br>

# Páginas do Dashboard Construído:
<img width="1246" height="696" alt="image" src="https://github.com/user-attachments/assets/61b1ba65-468c-428e-bb0d-09f0ebad981c" />

---

<img width="1250" height="701" alt="image" src="https://github.com/user-attachments/assets/1d72a894-2232-4a6a-baef-f432349e03f7" />

---

<img width="1246" height="698" alt="image" src="https://github.com/user-attachments/assets/fdf6bc16-becb-4e46-ae9a-bb3794494aac" />

---

## Dicas de Ferramenta:
<img width="360" height="275" alt="image" src="https://github.com/user-attachments/assets/be3a4a76-2501-4776-80b1-b9a425d243b3" />
<img width="364" height="275" alt="image" src="https://github.com/user-attachments/assets/c6ab3db1-4ada-49dc-b673-78e7f5dc902a" />
<br>

# Técnica de Análise:
Análise Exploratória no Dashboard em Power BI, utilizando recursos estatísticos para observar padrões e tendências para geração de insights<br>

# Insights extraídos:
[Imagens da apresentação] (Em construção)<br>
<br>

# Análises possíveis caso eu tivesse mais tempo:

### 1. Segmentação RFM (Recência, Frequência e Valor Monetário):
**1.1. Monetário   (M)**: Permitiria uma classificação mais precisa de LTV (Lifetime Value);<br>
**1.2. Frequência  (F)**: Ajudaria a distinguir o "campeão" (muitas compras, alto gasto) do "cliente premium" (poucas compras, altíssimo gasto);<br>
**1.3. Recência    (R)**: Nesse caso não seria útil, pois temos dados transacionais somente de um período de 3 meses.<br>
<br>

### 2. Análises Estatísticas:

#### 2.1. Análise de Cesta de Mercado (Market Basket Analysis - MBA)
**Método**: Análise de Regras de Associação (Conceitualmente, Algoritmo Apriori).<br>
**Objetivo**: Identificar grupos de produtos que são frequentemente comprados juntos.<br>
**Ação**: Calcular a Apoio (frequência de ocorrência conjunta) e a Confiança (probabilidade de comprar o item B se o item A foi comprado).<br>
**Insight**: "Se um cliente compra uma Calça Jeans, ele compra Meias Coloridas em 60% das vezes." Isso informa o cross-selling no PDV e o bundling (pacotes) de produtos.<br>
<br>

#### 2.2. Análise de Elasticidade-Preço da Demanda (PED)
**Método**: Regressão (ou análise de correlação segmentada);<br>
**Objetivo**: Medir a sensibilidade do cliente ao desconto;<br>
**Ação**: Analisar como o volume de venda (QUANTIDADE) de um produto específico muda em relação ao desconto concedido (VALOR_UNITARIO_VENDA_PRODUTO vs PRECO_TABELA);<br>
**Insight**: Identificar produtos que são elásticos (respondem muito bem ao desconto) e inelásticos (vendem a mesma quantidade, mesmo sem desconto, permitindo maior margem de lucro).<br>
<br>
