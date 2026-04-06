# Casos de teste

Este documento reúne os principais casos de teste do projeto **IoT Heartbeat Monitor**, estabelecendo a relação entre funcionalidades esperadas e comportamentos observáveis na simulação.

## 1. TST-001 – Monitoramento dos sinais cardíacos

**Objetivo:** verificar se o sistema realiza a leitura contínua do sinal e publica os valores no tópico MQTT adequado.

- **Caso de uso associado:** monitorar sinais cardíacos;
- **Entrada:** variação do potenciômetro em faixa normal;
- **Resultado esperado:** publicação periódica em `iot/monitor/ecg`, sem acionamento de LED ou buzzer.

## 2. TST-002 – Detecção de anomalia

**Objetivo:** verificar se o sistema identifica valores fora da faixa segura.

- **Caso de uso associado:** detectar anomalia nos sinais cardíacos;
- **Entrada:** valor abaixo de `500` ou acima de `2000`;
- **Resultado esperado:** reconhecimento da condição anormal pelo firmware.

## 3. TST-003 – Acionamento do LED

**Objetivo:** verificar se o LED é acionado imediatamente após a identificação da anomalia.

- **Caso de uso associado:** acionar atuadores locais;
- **Entrada:** manutenção do valor fora da faixa segura;
- **Resultado esperado:** LED ligado de forma imediata.

## 4. TST-004 – Acionamento do buzzer

**Objetivo:** verificar se o buzzer é acionado quando a anomalia persiste por aproximadamente dois segundos.

- **Caso de uso associado:** acionar atuadores locais;
- **Entrada:** permanência do valor em condição anormal por tempo suficiente;
- **Resultado esperado:** buzzer ativado e manutenção do alerta enquanto persistir a anomalia.

## 5. TST-005 – Envio de alerta via MQTT

**Objetivo:** verificar se o sistema publica uma mensagem de alerta no tópico apropriado quando a condição crítica persiste.

- **Caso de uso associado:** enviar alerta remoto;
- **Entrada:** anomalia prolongada;
- **Resultado esperado:** publicação em `iot/monitor/alert` com mensagem de alerta.

## 6. TST-006 – Controle do LED via MQTT

**Objetivo:** verificar o funcionamento do controle manual do LED.

- **Caso de uso associado:** controlar atuadores via MQTT;
- **Comandos testados:** `LED_ON`, `LED_OFF` e `LED_AUTO`;
- **Resultado esperado:** alteração correta do estado do LED conforme o comando recebido.

## 7. TST-007 – Controle do buzzer via MQTT

**Objetivo:** verificar o funcionamento do controle manual do buzzer.

- **Caso de uso associado:** controlar atuadores via MQTT;
- **Comandos testados:** `BUZZER_ON`, `BUZZER_OFF` e `BUZZER_AUTO`;
- **Resultado esperado:** alteração correta do estado do buzzer conforme o comando recebido.

## 8. TST-008 – Reset do sistema

**Objetivo:** verificar se o comando `RESET` faz o sistema retornar ao modo automático.

- **Caso de uso associado:** resetar sistema;
- **Entrada:** envio do comando `RESET` após alterações manuais;
- **Resultado esperado:** desativação do controle manual, desligamento dos atuadores e retorno à lógica automática.

## 9. Ambiente de teste

Os testes podem ser realizados no ambiente descrito a seguir:

- **simulação do circuito:** Wokwi;
- **execução do firmware:** arquivo `wokwi/sketch.ino`;
- **monitoramento e envio de mensagens:** MQTTX;
- **broker:** `test.mosquitto.org`.

## 10. Observação

Por se tratar de um protótipo acadêmico, os testes têm caráter funcional e demonstrativo, permitindo validar o comportamento esperado do sistema em ambiente controlado de simulação.
