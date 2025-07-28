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


