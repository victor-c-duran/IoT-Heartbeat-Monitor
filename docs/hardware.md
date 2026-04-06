# Hardware utilizado

Este documento descreve os componentes empregados no projeto **IoT Heartbeat Monitor** e apresenta as conexões adotadas na simulação.

## 1. Microcontrolador

O projeto utiliza um **ESP32 DevKit V1**, responsável por:

- realizar a leitura analógica do sinal simulado;
- controlar os atuadores de saída;
- conectar-se à rede Wi-Fi;
- comunicar-se com o broker MQTT.

### Pinos utilizados

- `ECG_PIN` → **GPIO 34**
- `LED_PIN` → **GPIO 25**
- `BUZZER_PIN` → **GPIO 26**

## 2. Sinal cardíaco simulado

Na simulação do Wokwi, o sinal cardíaco é representado por um **potenciômetro**. Esse componente permite variar manualmente o valor lido pelo conversor analógico-digital do ESP32.

### Ligações do potenciômetro

- terminal lateral → `3.3V`
- terminal central → `GPIO 34`
- terminal lateral oposto → `GND`

## 3. LED de alerta

O **LED** representa o alerta visual do sistema. Ele é acionado automaticamente quando o valor do sinal sai da faixa segura, salvo quando houver override manual via MQTT.

### Ligação do LED

- ânodo → resistor de `220Ω` → `GPIO 25`
- cátodo → `GND`

## 4. Buzzer

O **buzzer ativo** é utilizado como alerta sonoro. Seu acionamento automático ocorre quando a anomalia persiste por aproximadamente dois segundos, exceto em situações de override manual.

### Ligação do buzzer

- terminal positivo → `GPIO 26`
- terminal negativo → `GND`

## 5. Alimentação

Na simulação, a alimentação do circuito é realizada virtualmente. Em contexto físico, o ESP32 pode ser alimentado por USB, enquanto os demais componentes recebem energia a partir dos pinos da placa, conforme a necessidade do circuito.

## 6. Observações

- o ambiente Wokwi reproduz o comportamento lógico do circuito, mas não substitui cuidados de segurança elétrica exigidos em aplicações reais;
- em um cenário real de monitoramento cardíaco, o uso de sensores biomédicos exigiria proteção, isolamento e validação adicionais;
- no projeto acadêmico, o potenciômetro cumpre a função de gerar valores variáveis para testes e demonstração do comportamento do sistema.
