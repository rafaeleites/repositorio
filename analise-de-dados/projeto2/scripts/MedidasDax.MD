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