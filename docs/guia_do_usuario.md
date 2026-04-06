# Guia do Usuário – IoT Heartbeat Monitor

## 1. Apresentação

O presente guia do usuário tem como finalidade apresentar as instruções necessárias para utilização do sistema **IoT Heartbeat Monitor – Scenario 2 (MQTT)**, desenvolvido para monitoramento remoto de sinais cardíacos simulados em ambiente de Internet das Coisas. O sistema foi implementado com base em um **ESP32**, utilizando um **potenciômetro** para simulação do sinal de ECG, além de um **LED** e um **buzzer** como atuadores locais de alerta.

A solução permite a leitura contínua do sinal cardíaco, a identificação de valores fora da faixa considerada segura, a emissão de alertas visuais e sonoros e a comunicação remota via protocolo **MQTT**. Também é possível realizar o controle manual dos atuadores por meio de comandos enviados por um cliente MQTT externo.

Este documento foi elaborado para facilitar a reprodução e a demonstração do projeto, permitindo que qualquer usuário compreenda o funcionamento geral do sistema, execute a simulação no **Wokwi** e acompanhe os dados publicados por meio de um cliente MQTT, como o **MQTTX**.

---

## 2. Objetivo do sistema

O sistema foi projetado para:

- monitorar continuamente um sinal cardíaco simulado;
- detectar anomalias com base em limites pré-definidos;
- acionar alertas locais por LED e buzzer;
- publicar valores e alertas em um broker MQTT;
- permitir o envio de comandos remotos para controle manual dos atuadores.

A arquitetura do projeto representa um fluxo IoT completo, envolvendo sensoriamento, processamento local, comunicação em rede e interação remota por meio de tópicos MQTT.

---

## 3. Requisitos para utilização

Para a execução do sistema, são necessários os seguintes recursos:

- acesso à internet;
- navegador web atualizado;
- acesso ao simulador **Wokwi**;
- um cliente MQTT, preferencialmente **MQTTX**, para monitoramento e envio de comandos;
- arquivos do projeto disponíveis no repositório GitHub.

---

## 4. Estrutura do projeto

No repositório do projeto, os principais arquivos encontram-se organizados da seguinte forma:

```text
.
├─ README.md
├─ docs/
│  ├─ arquitetura.md
│  ├─ comunicacao_mqtt.md
│  ├─ guia_do_usuario.md
│  └─ hardware.md
└─ wokwi/
   ├─ diagram.md
   ├─ guia_de_simulacao.json
   ├─ libraries.txt
   └─ sketch.ino
```

Essa estrutura reúne a documentação técnica, os arquivos de simulação e o código-fonte utilizado pelo ESP32.

---

## 5. Componentes principais do sistema

O projeto utiliza os seguintes componentes:

- **ESP32 DevKit V1**: microcontrolador responsável pela lógica principal do sistema;
- **potenciômetro**: utilizado no Wokwi para simular o sinal de ECG;
- **LED vermelho**: indica visualmente a ocorrência de anomalia;
- **buzzer ativo**: emite alerta sonoro quando a anomalia persiste;
- **broker MQTT público**: `test.mosquitto.org`;
- **cliente MQTT externo**: MQTTX ou similar.

No código-fonte, os pinos utilizados são os seguintes:

- `GPIO 34` → entrada analógica do sinal de ECG;
- `GPIO 25` → saída digital para o LED;
- `GPIO 26` → saída digital para o buzzer.

---

## 6. Funcionamento geral do sistema

O sistema opera de acordo com o seguinte fluxo:

1. o ESP32 é inicializado;
2. os pinos de entrada e saída são configurados;
3. o dispositivo tenta se conectar à rede Wi‑Fi do Wokwi;
4. após a conexão, o ESP32 conecta-se ao broker MQTT;
5. o sistema passa a ler continuamente o valor analógico do sinal simulado;
6. cada leitura é publicada no tópico MQTT correspondente;
7. caso o valor esteja fora da faixa segura, o LED é acionado imediatamente;
8. se a condição anormal persistir por aproximadamente 2 segundos, o buzzer é ativado e um alerta é publicado;
9. o sistema permanece apto a receber comandos remotos via MQTT.

Os limites configurados para detecção de anomalia são:

- valor mínimo seguro: **500**;
- valor máximo seguro: **2000**.

Valores abaixo ou acima dessa faixa são interpretados como anormais.

---

## 7. Como executar o projeto no Wokwi

### 7.1 Acesso ao simulador

1. Acessar o site oficial do **Wokwi**.
2. Criar um novo projeto baseado em **ESP32 Arduino**.

### 7.2 Importação dos arquivos

Na pasta `wokwi/` do repositório, encontram-se os arquivos necessários para a simulação:

- `diagram.json`;
- `libraries.txt`;
- `sketch.ino`.

Para montar corretamente o ambiente:

1. carregar o arquivo `diagram.json` no projeto do Wokwi;
2. carregar o arquivo `libraries.txt`;
3. substituir o conteúdo do arquivo principal do projeto pelo código disponível em `wokwi/sketch.ino`.

### 7.3 Inicialização da simulação

Após a importação dos arquivos:

1. clicar no botão **Play** do Wokwi;
2. observar a inicialização do sistema no **Serial Monitor**;
3. verificar a conexão do ESP32 com a rede Wi‑Fi virtual e com o broker MQTT;
4. acompanhar a leitura contínua dos valores do sensor simulado.

---

## 8. Interpretação da simulação

Durante a execução, o potenciômetro pode ser girado para alterar o valor analógico lido pelo ESP32.

### 8.1 Situação normal

Quando o valor do sinal permanecer entre **500** e **2000**:

- o LED permanece desligado;
- o buzzer permanece desligado;
- os valores continuam sendo publicados no tópico de monitoramento.

### 8.2 Situação de anomalia

Quando o valor ficar abaixo de **500** ou acima de **2000**:

- o LED é ligado imediatamente;
- inicia-se a contagem de persistência da anomalia;
- após aproximadamente 2 segundos, o buzzer é acionado;
- uma mensagem de alerta é publicada no tópico MQTT de alertas.

Quando o valor retorna à faixa segura:

- o LED volta ao estado desligado, caso não haja controle manual ativo;
- o buzzer é desligado, caso não esteja em modo manual;
- o sistema reinicia a lógica automática de monitoramento.

---

## 9. Comunicação MQTT do projeto

A comunicação remota do sistema utiliza o broker público **test.mosquitto.org**, na porta **1883**, sem TLS, em caráter estritamente acadêmico.

### 9.1 Tópicos utilizados

O projeto utiliza os seguintes tópicos MQTT:

- `iot/monitor/ecg` → publicação dos valores do sinal cardíaco;
- `iot/monitor/alert` → publicação dos alertas de anomalia;
- `iot/monitor/cmd` → recebimento de comandos enviados por clientes externos.

### 9.2 Exemplo de publicação

No tópico `iot/monitor/ecg`, o sistema envia valores numéricos simples, como:

```text
1532
```

No tópico `iot/monitor/alert`, o sistema pode publicar uma mensagem como:

```text
Alert: abnormal heart measurement detected
```

---

## 10. Como monitorar o sistema com MQTTX

Para acompanhar o sistema remotamente, recomenda-se o uso do **MQTTX**.

### 10.1 Configuração da conexão

No MQTTX, deve-se criar uma nova conexão com os seguintes parâmetros:

- **Host:** `test.mosquitto.org`
- **Porta:** `1883`
- **Protocolo:** MQTT
- **TLS/SSL:** desativado

### 10.2 Inscrição nos tópicos

Após estabelecer a conexão, o usuário deve assinar os tópicos:

- `iot/monitor/ecg`
- `iot/monitor/alert`

Dessa forma, será possível visualizar tanto os valores do sinal quanto os alertas emitidos pelo sistema.

---

## 11. Comandos remotos disponíveis

O projeto permite o envio de comandos manuais ao ESP32 por meio do tópico:

```text
iot/monitor/cmd
```

Os comandos aceitos são os seguintes:

- `RESET`
- `LED_ON`
- `LED_OFF`
- `LED_AUTO`
- `BUZZER_ON`
- `BUZZER_OFF`
- `BUZZER_AUTO`

### 11.1 Função de cada comando

**RESET**  
Restaura o sistema para o modo automático, desativando controles manuais, apagando os atuadores e reinicializando a lógica de alerta.

**LED_ON**  
Liga manualmente o LED.

**LED_OFF**  
Desliga manualmente o LED.

**LED_AUTO**  
Retorna o LED ao modo automático, no qual seu estado dependerá da leitura do sensor.

**BUZZER_ON**  
Liga manualmente o buzzer.

**BUZZER_OFF**  
Desliga manualmente o buzzer.

**BUZZER_AUTO**  
Retorna o buzzer ao modo automático, no qual seu acionamento dependerá da persistência da anomalia.

---

## 12. Exemplos de uso

### 12.1 Cenário de monitoramento normal

1. iniciar a simulação no Wokwi;
2. manter o potenciômetro em posição intermediária;
3. observar os valores publicados em `iot/monitor/ecg`;
4. verificar que não há acionamento do LED nem do buzzer.

### 12.2 Cenário de anomalia automática

1. iniciar a simulação;
2. ajustar o potenciômetro para um valor abaixo de 500 ou acima de 2000;
3. verificar o acionamento imediato do LED;
4. aguardar cerca de 2 segundos;
5. confirmar o acionamento do buzzer e a publicação do alerta em `iot/monitor/alert`.

### 12.3 Cenário de controle manual

1. conectar um cliente MQTT ao broker;
2. publicar `LED_ON` em `iot/monitor/cmd`;
3. verificar o acionamento do LED;
4. publicar `BUZZER_ON` e verificar o acionamento do buzzer;
5. publicar `RESET` para restaurar o funcionamento automático.

---

## 13. Limitações do projeto

Por se tratar de um protótipo acadêmico e de uma simulação em ambiente controlado, devem ser observadas as seguintes limitações:

- o sinal cardíaco é simulado por potenciômetro, não por um sensor médico real;
- o broker MQTT utilizado é público e não possui garantias de segurança ou disponibilidade;
- a comunicação ocorre sem criptografia TLS;
- o sistema não deve ser utilizado em ambientes clínicos reais.

Em aplicações médicas reais, seriam necessários recursos adicionais de segurança, autenticação, isolamento elétrico, calibração e validação clínica.

---

## 14. Considerações finais

O guia do usuário apresentado tem por objetivo facilitar a compreensão e a reprodução do projeto **IoT Heartbeat Monitor**, descrevendo desde a estrutura do repositório até a execução da simulação e o uso da comunicação MQTT. A solução demonstra, de forma prática, a integração entre sensoriamento, processamento embarcado, envio de dados em rede e controle remoto, reforçando o potencial das aplicações de Internet das Coisas no contexto do monitoramento remoto em saúde.

---

## 15. Link sugerido para disponibilização

Recomenda-se que este guia seja salvo no repositório GitHub com um nome como:

```text
docs/guia_do_usuario.md
```

Após isso, o link a ser inserido no trabalho poderá apontar diretamente para esse arquivo no GitHub.
