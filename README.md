# Sistema-Inteligente-de-Monitoramento-Ambiental-Urbano-com-loT-aplicado-ODS-11

## Descrição
Este projeto apresenta um sistema inteligente de monitoramento ambiental urbano utilizando Internet das Coisas (IoT) aplicado ao ODS 11 – Cidades e Comunidades Sustentáveis.  O sistema utiliza a plataforma ESP32 integrada ao protocolo MQTT para monitoramento em tempo real de temperatura, umidade e qualidade do ar.

## Funcionamento
O ESP32 realiza leituras dos sensores DHT22 e MQ-135 e envia os dados em formato JSON para o broker MQTT HiveMQ utilizando o protocolo MQTT sobre TCP/IP.

Quando os valores ultrapassam os limites críticos:
- Temperatura > 60°C
- Gás > 3000

o sistema aciona automaticamente:
- LED (alerta visual)
- Buzzer (alerta sonoro)

- ## Hardware Utilizado

- ESP32
- Sensor DHT22
- Sensor MQ-135
- LED
- Buzzer
- Simulador Wokwi

- ## Comunicação MQTT

Broker utilizado:
broker.hivemq.com

Tópico MQTT:
iot/sensores/dados

Protocolo:
MQTT sobre TCP/IP

## Como executar

1. Abrir o projeto no Wokwi
2. Instalar a biblioteca PubSubClient
3. Executar o código no ESP32
4. Conectar ao broker HiveMQ
5. Monitorar os dados MQTT em tempo real

## Simulação Wokwi

https://wokwi.com/projects/462494399325855745
