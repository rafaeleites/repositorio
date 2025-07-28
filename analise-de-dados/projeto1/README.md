# Dashboard de Vendas e Clientes

## Introdução
Este projeto foi desenvolvido para atender às necessidades de análise estratégica de uma empresa varejista, permitindo uma visão detalhada sobre vendas, comportamento de clientes e eficiência operacional. Através de uma dashboard interativa, é possível identificar padrões, tendências e oportunidades de melhoria, auxiliando na tomada de decisões baseadas em dados.

## Descrição do Projeto
Este projeto apresenta uma dashboard interativa desenvolvida para análise detalhada de vendas e comportamento de clientes ao longo de um período de 4 anos. A dashboard foi projetada para fornecer insights estratégicos, como:

- Total de produtos vendidos e desempenho por categorias e segmentos.
- Média de pedidos faturados por dia e análise de recorrência de clientes.
- Classificação de clientes com base em frequência de compras e volume de pedidos.
- Identificação de padrões sazonais, como meses e anos com maior volume de vendas.
- Avaliação do tempo médio de envio e eficiência dos tipos de entrega.

Combinando visualizações intuitivas e fórmulas DAX avançadas, o projeto permite uma compreensão abrangente dos dados, auxiliando na tomada de decisões baseadas em evidências.

## Objetivos
- Monitorar o desempenho de vendas por categorias, segmentos e regiões.
- Identificar os clientes com maior volume de compras e melhor recorrência.
- Analisar o tempo médio de envio e os tipos de entrega mais utilizados.
- Obter insights sobre os meses e anos com maior volume de vendas.

## Principais Métricas
1. **Total de Produtos Vendidos**: 1,1 Bilhão.
2. **Total de Clientes**: 793.
3. **Média de Produtos Vendidos por Dia**: 906,31 Mil.
4. **Média de Dias Entre Pedidos do Mesmo Cliente**: 230 Dias
5. **Média de Pedidos Faturados por Cliente**: 6,21.
6. **Média de Pedidos Faturados por Dia**: 7,97
7. **Média de Produtos Vendidos por Pedido**: 113,75 Mil.
8. **Tempo Médio para o Pedido ser Enviado**: 4 Dias

## Tecnologias Utilizadas
- **Ferramenta de Visualização**: Power BI.
- **Linguagem de Modelagem**: DAX (Data Analysis Expressions).

## Como Utilizar
Este projeto foi desenvolvido para ser integrado diretamente ao site da empresa. Para utilizá-lo, basta acessar a dashboard interativa e explorar as métricas e insights disponíveis. Não é necessário realizar instalações adicionais.

## Conclusão
Este projeto fornece uma base sólida para análise de dados no setor varejista, permitindo identificar oportunidades de melhoria e otimizar processos. Com a integração de fórmulas DAX avançadas e visualizações intuitivas, a dashboard se torna uma ferramenta essencial para decisões estratégicas baseadas em dados.

## Insights Obtidos
### Clientes
- **Clientes com mais produtos comprados**: Anna Gayman lidera com 24,9 Milhões.
- **Clientes com mais pedidos faturados**: Emily Phan lidera com 17 pedidos.
- **Clientes com melhor média de recorrência**: Mick Hernandez com 61,125 dias.

### Vendas
- **Top 5 Sub-Categorias com Mais Vendas**: Bookcases, Chairs, Phones, Tables, Binders.
- **Top 5 Cidades com Mais Vendas**: New York City, Houston, Los Angeles, Philadelphia, San Francisco.
- **Top 5 Estados com Mais Vendas**: California, Texas, New York, Pennsylvania, Florida.

### Entregas
- **Produtos Vendidos por Tipo de Entrega**:
  - Standard Class: 656,7 Milhões.
  - Second Class: 225,3 Milhões.
  - First Class: 182,3 Milhões.
  - Same Day: 50,5 Milhões.
- **Média de Dias para Envio**:
  - Standard Class: 5 dias.
  - Second Class: 3 dias.
  - First Class: 2 dias.
  - Same Day: 0 dias.

### Período
- **Meses com Mais Produtos Vendidos**: Novembro e Setembro lideram com 175,9 Milhões e 172,1 Milhões, respectivamente.
- **Produtos Vendidos por Ano**: 2018 foi o ano com maior volume de vendas, totalizando 351,9 Milhões.

### Categorias e Segmentos
- **Produtos Vendidos por Categoria**:
  - Furniture: 542,9 Milhões.
  - Technology: 321,3 Milhões.
  - Office Supplies: 250,5 Milhões.
- **Produtos Vendidos por Segmento**:
  - Consumer: 593,2 Milhões.
  - Corporate: 329,7 Milhões.
  - Home Office: 191,9 Milhões.

## Fórmulas DAX

### Classificação de Clientes

- **ClassificacaoPorPedidos**: Classifica os clientes com base no número de pedidos realizados:
  ```DAX
  ClassificacaoPorPedidos = 
  VAR Pedidos = [Pedidos por Cliente]
  RETURN
      SWITCH(
          TRUE(),
          ISBLANK(Pedidos), "Sem Pedidos", -- Clientes sem pedidos registrados
          Pedidos <= 6, "Poucos Pedidos", -- Clientes com pedidos abaixo ou igual à média
          Pedidos > 6 && Pedidos <= 11, "Intermediário", -- Clientes com pedidos acima da média, mas não muito altos
          Pedidos > 11, "Muitos Pedidos", -- Clientes com pedidos altos
          "Outros" -- Fallback para valores inesperados
      )
  ```

- **ClassificaçãoRecorrencia**: Classifica os clientes com base na frequência de compras:
  ```DAX
  ClassificaçãoRecorrencia = 
  VAR Intervalo = [MediaDiasEntreComprasIndividual]
  RETURN
      SWITCH(
          TRUE(),
          ISBLANK(Intervalo), "Compra Única", -- Clientes sem compras registradas
          Intervalo <= 150, "Alta Recorrência", -- Clientes que compram frequentemente
          Intervalo <= 400, "Média Recorrência", -- Clientes com compras moderadas
          Intervalo > 400, "Baixa Recorrência", -- Clientes com compras esporádicas
          "Outros" -- Fallback para valores inesperados
      )
  ```

### Cálculos de Envio

- **Dias_Envio**: Calcula a diferença em dias entre a data do pedido (`Order Date`) e a data de envio (`Ship Date`).
  ```DAX
  Dias_Envio = DATEDIFF('varejo'[Order Date], 'varejo'[Ship Date], DAY)
  ```

- **Média de Envio por Dia**: Calcula a média de envios por dia, dividindo o número total de linhas pela contagem distinta de datas de envio.
  ```DAX
  Média de Envio por Dia = 
  DIVIDE(
      COUNTROWS('varejo'),
      DISTINCTCOUNT('varejo'[Ship Date])
  )
  ```

### Métricas de Pedidos

- **Média de Pedidos por Cliente**: Calcula a média de pedidos por cliente, dividindo o número distinto de pedidos pelo número distinto de clientes.
  ```DAX
  Média de Pedidos por Cliente = 
  DIVIDE(
      DISTINCTCOUNT('varejo'[Order ID]),
      DISTINCTCOUNT('varejo'[Customer ID])
  )
  ```

- **Média de Pedidos por Dia**: Calcula a média de pedidos por dia, dividindo o número total de linhas pela contagem distinta de datas de pedido.
  ```DAX
  Média de Pedidos por Dia = 
  DIVIDE(
      COUNTROWS('varejo'),
      DISTINCTCOUNT('varejo'[Order Date])
  )
  ```

### Métricas de Produtos

- **Média de Produtos por Pedido**: Calcula a média de produtos vendidos por pedido, dividindo o total de vendas pelo número total de linhas.
  ```DAX
  Média de Produtos por Pedido = 
  DIVIDE(
      SUM('varejo'[Sales]),
      COUNTROWS('varejo')
  )
  ```

- **Média de Produtos Vendidos por Dia**: Calcula a média de produtos vendidos por dia, dividindo o total de vendas pela contagem distinta de datas de pedido.
  ```DAX
  Média de Produtos Vendidos por Dia = 
  DIVIDE(
      SUM('varejo'[Sales]),
      DISTINCTCOUNT('varejo'[Order Date])
  )
  ```

### Análise de Recorrência

- **MediaDiasEntreComprasGeral**: Calcula a média de dias entre compras para todos os clientes:
  ```DAX
  MediaDiasEntreComprasGeral = 
  AVERAGEX(
      ADDCOLUMNS(
          SUMMARIZE('varejo', 'varejo'[Customer ID]),
          "DiasEntreCompras",
              VAR ClienteID = 'varejo'[Customer ID]
              VAR DatasPedidos =
                  CALCULATETABLE(
                      VALUES('varejo'[Order Date]),
                      'varejo'[Customer ID] = ClienteID
                  )
              VAR NumeroPedidos = COUNTROWS(DatasPedidos)
              RETURN
                  IF(
                      NumeroPedidos > 1,
                      DIVIDE(
                          DATEDIFF(MINX(DatasPedidos, [Order Date]), MAXX(DatasPedidos, [Order Date]), DAY),
                          NumeroPedidos - 1
                      )
                  )
      ),
      [DiasEntreCompras]
  )
  ```

- **MediaDiasEntreComprasIndividual**: Calcula a média de dias entre compras para cada cliente individualmente:
  ```DAX
  MediaDiasEntreComprasIndividual = 
  DIVIDE(
      DATEDIFF(
          CALCULATE(MIN('varejo'[Order Date]), ALLEXCEPT('varejo', 'varejo'[Customer Name])),
          CALCULATE(MAX('varejo'[Order Date]), ALLEXCEPT('varejo', 'varejo'[Customer Name])),
          DAY
      ),
      CALCULATE(
          DISTINCTCOUNT('varejo'[Order ID]),
          ALLEXCEPT('varejo', 'varejo'[Customer Name])
      ) - 1,
      BLANK()
  )
  ```

### Contagem de Pedidos

- **Pedidos por Cliente**: Calcula o número distinto de pedidos para cada cliente.
  ```DAX
  Pedidos por Cliente = 
  CALCULATE(
      DISTINCTCOUNT('varejo'[Order ID]),
      ALLEXCEPT('varejo', 'varejo'[Customer Name])
  )
  ```


