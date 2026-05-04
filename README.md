# IoT Heartbeat Monitor

Este repositório contém o projeto **IoT Heartbeat Monitor**, desenvolvido na disciplina **Prática Profissional em Análise e Desenvolvimento de Sistemas**. O sistema simula o monitoramento de sinais cardíacos por meio de um circuito com ESP32 no Wokwi, identifica anomalias e envia dados e alertas via MQTT, além de permitir o controle remoto dos atuadores.

## Visão geral

O protótipo realiza as seguintes funções:

- leitura contínua de um sinal cardíaco simulado por potenciômetro;
- identificação de valores fora da faixa segura;
- acionamento automático de LED e buzzer em situações anormais;
- publicação de leituras e alertas em tópicos MQTT;
- recebimento de comandos remotos para controle manual dos atuadores;
- restauração do funcionamento automático por meio do comando `RESET`.

## Estrutura do repositório

```text
.
├─ README.md
├─ docs/
|  └─ img/
│     ├─ fluxograma.png
|     └─ projeto_wokwi.png
│  ├─ arquitetura.md
│  ├─ comunicacao_mqtt.md
│  ├─ guia_do_usuario.md
│  ├─ hardware.md
│  ├─ instalacao_execucao.md
│  └─ testes.md
└─ wokwi/
   ├─ README.md
   ├─ diagram.json
   ├─ libraries.txt
   └─ sketch.ino
```

## Documentação disponível

- [Guia do usuário](docs/guia_do_usuario.md)
- [Instalação e execução](docs/instalacao_execucao.md)
- [Arquitetura do sistema](docs/arquitetura.md)
- [Hardware utilizado](docs/hardware.md)
- [Comunicação MQTT](docs/comunicacao_mqtt.md)
- [Casos de teste](docs/testes.md)
- [Guia rápido da simulação no Wokwi](wokwi/README.md)
- [Imagens](docs/img/)

## Execução da simulação

O projeto foi estruturado para execução no **Wokwi**. Os arquivos da simulação encontram-se na pasta `wokwi/`.

Resumo do processo:

1. acessar o Wokwi;
2. criar um projeto ESP32 Arduino;
3. importar `diagram.json` e `libraries.txt`;
4. copiar o conteúdo de `wokwi/sketch.ino` para o editor do simulador;
5. iniciar a simulação e acompanhar o monitor serial;
6. utilizar um cliente MQTT externo, como o MQTTX, para observar os tópicos e enviar comandos.

O passo a passo completo está em [docs/instalacao_execucao.md](docs/instalacao_execucao.md).

## Comunicação MQTT

O sistema utiliza o broker público `test.mosquitto.org` e os seguintes tópicos:

- `iot/monitor/ecg` para publicação das leituras do sinal;
- `iot/monitor/alert` para publicação dos alertas;
- `iot/monitor/cmd` para recebimento de comandos remotos.

Os detalhes de payloads, fluxo de mensagens e comandos aceitos estão em [docs/comunicacao_mqtt.md](docs/comunicacao_mqtt.md).

## Hardware e simulação

O protótipo utiliza:

- **ESP32 DevKit V1**;
- **potenciômetro** simulando o sinal de ECG no Wokwi;
- **LED** para alerta visual;
- **buzzer** para alerta sonoro.

As ligações e observações de hardware estão em [docs/hardware.md](docs/hardware.md).

## Autores

- Alecsei Ivanov da Costa
- Víctor Céspedes Duran

## Licença

Projeto disponibilizado para fins acadêmicos.


## Iteração 2

Nesta iteração foram realizadas pequenas alterações na documentação do projeto com o objetivo de demonstrar o uso de branches e merge no controle de versão.