# consultoria_driva-tech_powerbi (Em Construção)
Consultoria fictícia realizada para uma empresa varejista de moda com 5 lojas físicas em Curitiba.<br>
**Objetivo**: Encontrar padrões e tendências e sugerir estratégias para otimizar a distribuição de produtos e promoções nos PDV's, visando aumento de vendas e melhoria na satisfação do cliente.<br>
O cliente forneceu dados das vendas das lojas e dados demográficos dos consumidores.

## Desafio
**Contexto**: Um varejista de moda, cliente da DrivaTech, busca entender melhor o desempenho de suas lojas físicas para otimizar estratégias de distribuição de produtos e promoções, visando aumento das vendas e melhoria na satisfação do cliente. O cliente forneceu dados das vendas das lojas e dados demográficos dos consumidores.<br>
<br>
**Esquema de Dados**:<br>
<img width="1261" height="492" alt="image" src="https://github.com/user-attachments/assets/0a27a046-5d15-48eb-8bc1-4530005da97c" />
<br>
**Desafio**:
1. Examinar os dados para identificar padrões de vendas e diferenças regionais;<br>
2. Aplicar técnicas básicas de segmentação para identificar diferentes perfis de consumidores. Analisar suas preferências de compra para entender quais produtos ou promoções são mais eficazes para cada segmento;<br>
3. Elaborar recomendações práticas sobre como ajustar a distribuição de produtos e as estratégias de marketing para melhor atender aos diferentes segmentos de clientes, baseando-se nos insights extraídos das análises anteriores.<br>

## Metodologia
**Origem dos Dados**: Fictícios. Gerados de forma randômica, porém seguindo modelo de consultoria para aplicação de conceitos e estudo.<br>
**Transformação e Carregamento**: carregamento das planilhas normalizadas em Power BI e Transformação em Power Query para limpeza, criação de novas colunas, medidas e criação da tabela calendário utilizando o código abaixo:<br>
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
**Modelagem de Dados**: Star Schema em Power BI, a partir de 5 planilhas de Excel normalizadas<>br
[Inserir Imagem das relações entre tabelas do Power BI]<br>
**Técnica de Análise**: Criação de dashboard em Power BI utilizando fórmulas DAX avançadas e Análise Exploratória<br>
[Imagens do Dashboard]<br>
**Insights extraídos**:<br>
[Imagens da apresentação]<br>
**Análises possíveis**:<br>
