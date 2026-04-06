# Instalação e execução

Este documento apresenta o procedimento para executar o projeto **IoT Heartbeat Monitor** no ambiente de simulação Wokwi e testá-lo com um cliente MQTT externo.

## 1. Requisitos

Para reproduzir o projeto, são necessários:

- acesso à internet;
- navegador web atualizado;
- conta no Wokwi ou uso do ambiente público de simulação;
- cliente MQTT externo, como o **MQTTX**;
- acesso a este repositório e aos arquivos da pasta `wokwi/`.

## 2. Arquivos necessários

Os arquivos utilizados na simulação estão localizados em `wokwi/`:

- `diagram.json`
- `libraries.txt`
- `sketch.ino`

## 3. Execução no Wokwi

### 3.1 Criação do projeto

1. acessar o site do **Wokwi**;
2. selecionar a opção de criação de novo projeto;
3. escolher o modelo **ESP32 Arduino**.

### 3.2 Importação dos arquivos

Após a criação do projeto:

1. remover ou substituir os arquivos padrão gerados pelo Wokwi, quando necessário;
2. importar os arquivos `diagram.json` e `libraries.txt` da pasta `wokwi/`;
3. abrir o arquivo `sketch.ino` do projeto criado no simulador;
4. copiar o conteúdo de `wokwi/sketch.ino` deste repositório e colá-lo no editor do Wokwi.

## 4. Inicialização da simulação

Depois de importar os arquivos e inserir o código-fonte:

1. clicar em **Play** para iniciar a simulação;
2. abrir o **Serial Monitor**;
3. observar as mensagens de inicialização do sistema.

Espera-se visualizar no monitor serial:

- tentativa de conexão ao Wi-Fi `Wokwi-GUEST`;
- conexão ao broker MQTT `test.mosquitto.org`;
- assinatura do tópico `iot/monitor/cmd`;
- impressão periódica dos valores analógicos lidos.

## 5. Interação com o circuito

A simulação utiliza um potenciômetro para representar o sinal cardíaco.

### 5.1 Situação normal

Quando o valor lido permanece entre `500` e `2000`:

- o LED deve permanecer desligado;
- o buzzer deve permanecer desligado;
- o valor deve ser publicado em `iot/monitor/ecg`.

### 5.2 Situação anormal

Quando o valor fica abaixo de `500` ou acima de `2000`:

- o LED deve ser acionado imediatamente;
- se a condição persistir por cerca de dois segundos, o buzzer deve ser acionado;
- um alerta deve ser publicado em `iot/monitor/alert`.

## 6. Teste com MQTTX

### 6.1 Configuração da conexão

No MQTTX, criar uma conexão com os seguintes parâmetros:

- **Host:** `test.mosquitto.org`
- **Porta:** `1883`
- **Protocolo:** MQTT sem TLS

### 6.2 Tópicos para monitoramento

Inscrever-se nos tópicos abaixo:

- `iot/monitor/ecg`
- `iot/monitor/alert`

### 6.3 Envio de comandos

Publicar no tópico `iot/monitor/cmd` os comandos desejados, por exemplo:

- `LED_ON`
- `LED_OFF`
- `LED_AUTO`
- `BUZZER_ON`
- `BUZZER_OFF`
- `BUZZER_AUTO`
- `RESET`

## 7. Resultados esperados

Ao longo da execução, os seguintes comportamentos devem ser observados:

- publicação periódica dos valores analógicos;
- acionamento do LED em condição anormal;
- acionamento do buzzer após persistência da anomalia;
- envio de alerta MQTT quando o buzzer for ativado;
- resposta imediata aos comandos enviados pelo cliente MQTT;
- retorno ao modo automático ao receber `RESET`.

## 8. Observações finais

- o broker utilizado é público, portanto falhas ocasionais de conectividade podem ocorrer;
- o ambiente Wokwi é suficiente para validação acadêmica e demonstração do protótipo;
- em uma aplicação real, o projeto exigiria refinamentos de segurança, confiabilidade e integração com hardware biomédico apropriado.
