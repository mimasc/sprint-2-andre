# sprint-2-andre: mirella mascarenhas; guilherme tamai; caio castelão; vitor komura; andre nobrega; andre gouveia

# Sistema de Controle de Energia Residencial
# Um sistema interativo em Python para simular e gerenciar o consumo de energia em uma residência. Ideal para fins educacionais, demonstrações ou prototipagem de lógica de economia de energia.

#Funcionalidades:

# Monitoramento do consumo total de energia
# Simulação em tempo real da descarga da bateria
# Visualização do status de todos os dispositivos
# Modo de economia de energia (desativa dispositivos não essenciais)
# Ligar e desligar dispositivos específicos
# Interface via linha de comando com comandos simples

#Como funciona:

#O sistema gerencia uma lista de dispositivos domésticos, cada um com:

#Prioridade (1 = essencial, 2 = importante, 3 = opcional)
#Status (ligado/desligado)
#Consumo de energia (em kWh)

#Estrutura do sistema: 

SistemaEnergia
├── cargas (dict)
│   ├── geladeira (prioridade 1, 0.5 kWh)
│   ├── roteador (prioridade 1, 0.1 kWh)
│   ├── ar_condicionado (prioridade 2, 1.5 kWh)
│   ├── tv (prioridade 2, 0.3 kWh)
│   └── iluminacao (prioridade 3, 0.2 kWh)
├── nivel_bateria (float)
├── simulando (bool)
└── Métodos:
    ├── mostrar_status()
    ├── comando_virtual()
    ├── _simular_consumo()
    ├── _economizar_energia()
    ├── _ativar_tudo()
    ├── _alterar_status_dispositivo()
    └── _exibir_comandos()

#Diagrama de Arquitetura:

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

#Diagrama de Fluxo – Execução Principal:

Início
  │
  ▼
Instancia SistemaEnergia
  │
  ├─► mostrar_status()
  │
  ▼
Loop até "sair":
  │
  ├─► Lê comando do usuário
  │
  ├─► comando_virtual(cmd)
  │     ├─ "economizar energia" → _economizar_energia()
  │     ├─ "ativar tudo"        → _ativar_tudo()
  │     ├─ "simular consumo"    → _simular_consumo()
  │     ├─ "status"             → mostrar_status()
  │     ├─ "ligar [nome]"       → _alterar_status_dispositivo()
  │     ├─ "desligar [nome]"    → _alterar_status_dispositivo()
  │     └─ "ajuda"              → _exibir_comandos()
  │
  └─► Imprime resposta (se houver)


#Diagrama de Fluxo – Simulação de Consumo:

_simular_consumo()
  │
  ├─ Verifica se já está simulando
  ├─ Define simulando = True
  │
  ▼
Loop enquanto simulando e bateria > 0:
  │
  ├─ Calcula consumo atual (somente cargas ativas)
  ├─ Reduz nível da bateria (consumo * 0.1)
  ├─ Mostra horário, consumo, bateria
  └─ Aguarda 3 segundos
  ▼
Se bateria <= 0:
  └─ Mostra aviso de bateria esgotada
