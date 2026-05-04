# Guia rápido da simulação no Wokwi

Este documento apresenta um roteiro resumido para executar o projeto **IoT Heartbeat Monitor** no simulador **Wokwi**.

## Arquivos da pasta `wokwi/`

A pasta contém os arquivos necessários para a simulação:

- `diagram.json`: definição do circuito eletrônico;
- `libraries.txt`: bibliotecas utilizadas no ambiente;
- `sketch.ino`: código-fonte principal do ESP32.

## Passo a passo resumido

1. acessar o site do Wokwi;
2. criar um projeto do tipo **ESP32 Arduino**;
3. importar `diagram.json` e `libraries.txt`;
4. abrir o arquivo `sketch.ino` do projeto no Wokwi;
5. substituir o conteúdo padrão pelo código de `wokwi/sketch.ino` deste repositório;
6. iniciar a simulação;
7. observar o monitor serial e interagir com o potenciômetro;
8. utilizar um cliente MQTT externo, se desejado, para acompanhar tópicos e enviar comandos.

## Testes rápidos sugeridos

- variar o potenciômetro para observar a mudança nas leituras publicadas;
- deslocar o valor para fora da faixa segura e verificar o acionamento do LED;
- manter a anomalia por mais de dois segundos para verificar o acionamento do buzzer e o envio de alerta MQTT;
- publicar comandos em `iot/monitor/cmd` para testar o controle manual.

## Referência complementar

Para instruções completas, consultar:

- `docs/instalacao_execucao.md`
- `docs/comunicacao_mqtt.md`
