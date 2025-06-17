# sprint-2-andre: mirella mascarenhas; guilherme tamai; caio castelão; vitor komura; andre nobrega; andre gouveia
# Diagrama de Arquitetura Funcional
┌──────────────────────────────┐
│      Usuário / Assistente    │
└────────────┬─────────────────┘
             │
             ▼
   comando_virtual(comando)
             │
    ┌────────┼──────────┐
    ▼        ▼          ▼
_economizar_energia()   _ativar_tudo()
    ▼                   │
 _atualizar_sistema()   │
                        ▼
         _simular_ciclo() (modo automático)
                        │
        ┌───────────────┴───────────────┐
        ▼                               ▼
_analisar_consumo()           _salvar_dados()
                                        │
                              ┌─────────┴──────────┐
                              ▼                    ▼
                 dados_energia.json         log_energia.csv

# Diagrama de Fluxo __main__

Início
  │
  ▼
Instancia SistemaEnergiaInteligente
  │
  ├──► Exibe Status Inicial
  │
  ├──► Exibe Lista de Comandos
  │
  ▼
Loop: Aguardando Comando
  │
  ├───► Se comando == "sair"
  │         └──► Encerra
  │
  └───► sistema.comando_virtual(cmd)
              │
              ├── Atualiza Sistema
              ├── Salva Dados
              └── (Exibe status ou executa ação)

# Diagrama de Fluxo _simular_ciclo()

_simular_ciclo()
  │
  ├──► _simular_geracao_solar()
  │
  ├──► Calcula saldo = geração - consumo
  │
  ├──► Atualiza nível da bateria com saldo
  │
  ├──► Se bateria <= 30%
  │         └── Desliga cargas de prioridade > 2
  │
  ├──► Se bateria >= 80%
  │         └── Liga cargas de prioridade <= 2
  │
  ├──► _atualizar_sistema()
  └──► analisar_consumo()

