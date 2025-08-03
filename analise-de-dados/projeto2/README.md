# Dashboard de Análise de Vício em Redes Sociais

## Introdução
Este projeto foi desenvolvido para analisar o impacto do uso de redes sociais na vida de estudantes, abrangendo aspectos como saúde mental, desempenho acadêmico e dinâmicas de relacionamento. Através de uma dashboard interativa, é possível identificar padrões de uso, dependência e conflitos relacionados ao uso de redes sociais, auxiliando na tomada de decisões baseadas em dados.

## Descrição do Projeto
Este projeto apresenta uma dashboard interativa desenvolvida para análise detalhada do comportamento de estudantes em redes sociais. A dashboard foi projetada para fornecer insights estratégicos, como:

- Média de uso diário de redes sociais e impacto no desempenho acadêmico.
- Classificação de estudantes por perfil de risco e dependência.
- Correlação entre saúde mental, horas de sono e uso de redes sociais.
- Preferências de plataformas e padrões de uso por faixa etária e gênero.
- Identificação de conflitos relacionados ao uso de redes sociais.

Combinando visualizações intuitivas e fórmulas DAX avançadas, o projeto permite uma compreensão abrangente dos dados, auxiliando na tomada de decisões baseadas em evidências.

## Objetivos
- Monitorar o impacto do uso de redes sociais na saúde mental e desempenho acadêmico.
- Identificar padrões de dependência e perfil de risco entre estudantes.
- Analisar a relação entre uso de redes sociais e horas de sono.
- Obter insights sobre preferências de plataformas e dinâmicas de relacionamento.

## Principais Métricas
1. **Total de Estudantes**: 705.
2. **Média de Idade**: 20,66 anos.
3. **Média de Score Mental**: 6,23.
4. **Média de Uso Diário (Horas)**: 4,92.
5. **Score de Dependência**: 6,44.
6. **Média de Sono (Horas)**: 6,87.

## Tecnologias Utilizadas
- **Ferramenta de Visualização**: Power BI.
- **Linguagem de Modelagem**: DAX (Data Analysis Expressions).

## Como Utilizar
Este projeto foi desenvolvido para ser integrado diretamente ao site da empresa ou utilizado em apresentações acadêmicas. Para utilizá-lo, basta acessar a dashboard interativa e explorar as métricas e insights disponíveis. Não é necessário realizar instalações adicionais.

## Conclusão
Este projeto fornece uma base sólida para análise de dados sobre o impacto das redes sociais na vida de estudantes, permitindo identificar oportunidades de intervenção e otimizar estratégias de conscientização. Com a integração de fórmulas DAX avançadas e visualizações intuitivas, a dashboard se torna uma ferramenta essencial para decisões estratégicas baseadas em dados.

## Insights Obtidos
### Estudantes
- **Plataforma Mais Utilizada**: Instagram lidera com 249 usuários.
- **Classificação por Perfil de Risco**: 544 estudantes classificados como risco moderado.
- **Score de Dependência por Conflitos**: Maior concentração entre scores 6 e 8.

### Saúde Mental e Sono
- **Score de Saúde Mental por Uso Diário**: Média de 6,23.
- **Score de Dependência por Sono**: Estudantes com menos horas de sono apresentam maior dependência.

### Desempenho Acadêmico
- **Impacto no Desempenho Acadêmico**: 35,74% dos estudantes relataram impacto negativo.

### Demografia
- **Gênero**: 50,07% masculino e 49,93% feminino.
- **Estado Civil**: Maior concentração de estudantes solteiros (384).

## Fórmulas DAX
As fórmulas DAX utilizadas neste projeto serão adicionadas em breve.

### Classificação de Usuários

- **Tipo de Usuário**: Classifica os estudantes com base em critérios de uso diário de redes sociais, saúde mental, dependência e horas de sono.
  ```DAX
  Tipo de Usuário = 
  VAR Uso = 'Students_Social_Media'[UsoDiarioHoraRedeSocial]
  VAR SaudeMental = 'Students_Social_Media'[ScoreSaudeMental]
  VAR Dependencia = 'Students_Social_Media'[ScoreDependencia]
  VAR Sono = 'Students_Social_Media'[HorasSonoNoite] 

  RETURN
      IF (
          // 1. Usuários de Alto Risco
          // Critério: Uso SIGNIFICATIVAMENTE ALTO, Saúde Mental SIGNIFICATIVAMENTE BAIXA, Dependência SIGNIFICATIVAMENTE ALTA E SONO SIGNIFICATIVAMENTE BAIXO
          Uso >= 4.5 && SaudeMental <= 4.5 && Dependencia >= 7 && Sono <= 5.5,
          "Usuários de Alto Risco",
          IF (
              // 2. Usuário Oculto de Risco
              // Critério: Uso MODERADO/BAIXO, Saúde Mental SIGNIFICATIVAMENTE BAIXA, Dependência MODERADA E SONO BAIXO/MODERADO
              Uso >= 1 && Uso <= 5 && SaudeMental <= 4.5 && Dependencia >= 5 && Dependencia <= 7.5 && Sono > 4 && Sono <= 6.5,
              "Usuário Oculto de Risco",
              IF (
                  // 3. Usuários Equilibrados
                  // Critério: Uso MODERADO, Saúde Mental SIGNIFICATIVAMENTE ALTA, Dependência SIGNIFICATIVAMENTE BAIXA E SONO SIGNIFICATIVAMENTE ALTO
                  Uso >= 3.5 && Uso <= 6 && SaudeMental >= 7 && Dependencia <= 4.5 && Sono >= 7,
                  "Usuário Equilibrado",
                  IF (
                      // 4. Risco Moderado
                      // Critério: Captura os que sobraram e que apresentam PELO MENOS UM indicador em nível MODERADO de preocupação
                      (Uso > 5 && Uso <= 6) || (SaudeMental > 5 && SaudeMental <= 6.5) || (Dependencia > 4.5 && Dependencia <= 6) || (Sono > 5.5 && Sono < 7),
                      "Risco Moderado",
                      // 5. Neutro
                      // Qualquer combinação de scores que não se encaixe nas categorias acima.
                      "Neutro"
                  )
              )
          )
      )
  ```


