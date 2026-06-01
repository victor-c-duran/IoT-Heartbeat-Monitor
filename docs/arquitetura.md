# Arquitetura do sistema

Este documento descreve a arquitetura geral do projeto **IoT Heartbeat Monitor**, destacando os componentes envolvidos, o fluxo de funcionamento e a distribuição das responsabilidades entre dispositivo embarcado, comunicação em rede e aplicação de monitoramento.

## 1. Visão em camadas

### 1.1 Camada de dispositivo embarcado

A camada de borda é representada pelo **ESP32**, responsável pela execução da lógica principal do sistema. Nessa camada estão concentradas as seguintes funções:

- leitura analógica do sinal simulado no **GPIO 34**;
- verificação da faixa de normalidade do sinal;
- acionamento do **LED** no **GPIO 25**;
- acionamento do **buzzer** no **GPIO 26**;
- publicação dos dados e alertas via MQTT;
- recebimento de comandos remotos para controle manual.

### 1.2 Camada de comunicação

A comunicação é realizada por meio de **Wi-Fi**, utilizando **TCP/IP** e o protocolo **MQTT**. O broker adotado no projeto é o serviço público `test.mosquitto.org` ou privado `broker.hivemq.com`.

Essa camada viabiliza:

- envio periódico das leituras do sensor;
- envio de alertas em caso de anomalia prolongada;
- recepção de comandos enviados por um cliente remoto.

### 1.3 Camada de aplicação

A camada de aplicação é composta por um cliente MQTT externo, como o **MQTTX**, executado em um computador ou smartphone. Nela é possível:

- acompanhar os valores publicados em tempo real;
- observar o recebimento de alertas;
- enviar comandos para teste e controle dos atuadores;
- reinicializar o comportamento automático do sistema.

## 2. Fluxo de funcionamento

O funcionamento geral do sistema ocorre da seguinte forma:

1. o ESP32 inicializa os pinos do circuito;
2. o dispositivo tenta conexão com a rede Wi-Fi;
3. após a conexão, o sistema se conecta ao broker MQTT;
4. o tópico `iot/monitor/cmd` é assinado para recebimento de comandos;
5. o valor analógico do sinal é lido continuamente;
6. a leitura é comparada com os limites estabelecidos (`500` a `2000`);
7. em condição normal, o sistema publica o valor lido em `iot/monitor/ecg`;
8. em condição anormal, o LED é acionado imediatamente;
9. se a anomalia persistir por aproximadamente dois segundos, o buzzer é ativado e um alerta é enviado em `iot/monitor/alert`;
10. caso chegue um comando MQTT, o sistema aplica o controle manual correspondente;
11. ao receber `RESET`, o sistema retorna ao modo automático.

## 3. Modos de operação

O protótipo opera em dois modos complementares:

### 3.1 Modo automático

Nesse modo, LED e buzzer respondem à lógica de detecção de anomalias definida no firmware.

### 3.2 Modo manual

Nesse modo, o usuário pode alterar o estado dos atuadores por meio dos comandos MQTT, como `LED_ON`, `LED_OFF`, `BUZZER_ON` e `BUZZER_OFF`. O retorno ao comportamento automático pode ser realizado com `LED_AUTO`, `BUZZER_AUTO` ou `RESET`, conforme o caso.

## 4. Arquitetura de implantação

A arquitetura física do projeto pode ser resumida em três nós principais:

- **dispositivo do usuário**, com um cliente MQTT para monitoramento e envio de comandos;
- **broker MQTT**, responsável pela intermediação das mensagens;
- **ESP32**, que executa o firmware, lê o sinal, processa os dados e aciona os atuadores.

A comunicação entre o usuário e o ESP32 não ocorre de forma direta, mas mediada pelo broker MQTT.

## 5. Possíveis extensões

Entre as possíveis evoluções do projeto, destacam-se:

- substituição do potenciômetro por um sensor biomédico real, como o AD8232;
- criação de dashboard web para visualização contínua dos dados;
- armazenamento de histórico em banco de dados;
- uso de autenticação e criptografia na comunicação MQTT;
- integração com serviços em nuvem.
