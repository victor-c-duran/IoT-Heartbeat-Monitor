# Comunicação MQTT

Este documento apresenta a estrutura de comunicação MQTT utilizada no projeto **IoT Heartbeat Monitor**, incluindo broker, tópicos, payloads, comandos aceitos e recomendações de teste.

## 1. Broker utilizado

O projeto utiliza o broker público abaixo:

- **Host:** `test.mosquitto.org` ou `broker.hivemq.com`
- **Porta:** `1883`
- **Transporte:** TCP/IP
- **Segurança:** sem TLS, em ambiente acadêmico e de simulação

No código, a configuração do broker está definida da seguinte forma, por exemplo:

```cpp
const char* mqtt_server = "test.mosquitto.org";
```

## 2. Tópicos do projeto

### 2.1 Tópico de leituras

- **Tópico:** `iot/monitor/ecg`
- **Direção:** ESP32 → broker → cliente MQTT
- **Função:** publicar o valor analógico lido do sinal cardíaco simulado
- **Frequência aproximada:** 1 publicação por segundo
- **Formato do payload:** texto simples contendo um número inteiro

Exemplo:

```text
1532
```

### 2.2 Tópico de alertas

- **Tópico:** `iot/monitor/alert`
- **Direção:** ESP32 → broker → cliente MQTT
- **Função:** enviar alerta quando uma anomalia persiste por tempo suficiente para acionar o buzzer
- **Formato do payload:** texto simples

Exemplo:

```text
Alert: abnormal heart measurement detected
```

### 2.3 Tópico de comandos

- **Tópico:** `iot/monitor/cmd`
- **Direção:** cliente MQTT → broker → ESP32
- **Função:** permitir controle manual dos atuadores e retorno ao modo automático

## 3. Comandos aceitos

O firmware aceita os seguintes comandos via tópico `iot/monitor/cmd`:

- `RESET`
- `LED_ON`
- `LED_OFF`
- `LED_AUTO`
- `BUZZER_ON`
- `BUZZER_OFF`
- `BUZZER_AUTO`

### 3.1 Efeito dos comandos

- `RESET`: cancela controles manuais, desliga os atuadores e retorna o sistema ao modo automático;
- `LED_ON`: ativa o LED manualmente;
- `LED_OFF`: desativa o LED manualmente;
- `LED_AUTO`: devolve o LED à lógica automática;
- `BUZZER_ON`: ativa o buzzer manualmente;
- `BUZZER_OFF`: desativa o buzzer manualmente;
- `BUZZER_AUTO`: devolve o buzzer à lógica automática.

## 4. Fluxo de comunicação

O fluxo MQTT do projeto pode ser resumido da seguinte maneira:

1. o ESP32 conecta-se à rede Wi-Fi;
2. o ESP32 conecta-se ao broker MQTT;
3. o dispositivo assina o tópico `iot/monitor/cmd`;
4. o ESP32 publica periodicamente o valor lido em `iot/monitor/ecg`;
5. quando uma anomalia persiste, o sistema publica um alerta em `iot/monitor/alert`;
6. quando o usuário envia comandos, o ESP32 os processa e altera o comportamento dos atuadores.

## 5. Teste com cliente MQTT

Para testar a comunicação, pode-se utilizar o **MQTTX** ou outro cliente compatível.

### Configuração da conexão

- **Host:** `test.mosquitto.org` ou `broker.hivemq.com`
- **Porta:** `1883`
- **Client ID:** livre, desde que não entre em conflito com outro cliente conectado

### Inscrições recomendadas

- `iot/monitor/ecg`
- `iot/monitor/alert`

### Publicações de teste

- tópico: `iot/monitor/cmd`
- payloads: `LED_ON`, `LED_OFF`, `BUZZER_ON`, `BUZZER_OFF`, `LED_AUTO`, `BUZZER_AUTO`, `RESET`

## 6. Observações importantes

- o broker utilizado é público, portanto não há garantia de disponibilidade contínua;
- em função do uso acadêmico, a comunicação foi mantida sem TLS;
- em aplicações reais, seria necessário empregar autenticação, criptografia e políticas de confiabilidade mais rígidas;
- o projeto utiliza uma abordagem suficiente para demonstração didática da comunicação MQTT em um cenário de IoT.
