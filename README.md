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

## Código Fonte

#include <WiFi.h>
#include <PubSubClient.h>
#include "DHT.h"

// =========================
// WIFI
// =========================

const char* ssid = "Wokwi-GUEST";
const char* password = "";

// =========================
// MQTT
// =========================

const char* mqtt_server = "broker.hivemq.com";

WiFiClient espClient;
PubSubClient client(espClient);

// =========================
// PINOS
// =========================

#define DHTPIN 4
#define DHTTYPE DHT22

#define LED_PIN 2
#define BUZZER_PIN 18
#define MQ135_PIN 34

DHT dht(DHTPIN, DHTTYPE);

// =========================
// WIFI
// =========================

void setup_wifi() {

  delay(10);

  Serial.println();
  Serial.print("Conectando ao WiFi... ");

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {

    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi conectado!");
}

// =========================
// MQTT
// =========================

void reconnect() {

  while (!client.connected()) {

    Serial.print("Conectando MQTT... ");

    if (client.connect("ESP32Client")) {

      Serial.println("conectado!");

    } else {

      Serial.print("Falhou: ");
      Serial.print(client.state());

      delay(2000);
    }
  }
}

// =========================
// SETUP
// =========================

void setup() {

  Serial.begin(115200);

  pinMode(LED_PIN, OUTPUT);
  pinMode(BUZZER_PIN, OUTPUT);

  dht.begin();

  setup_wifi();

  client.setServer(mqtt_server, 1883);

  Serial.println("Sistema iniciado!");
}

// =========================
// LOOP
// =========================

void loop() {

  if (!client.connected()) {
    reconnect();
  }

  client.loop();

  float temperatura = dht.readTemperature();
  float umidade = dht.readHumidity();

  int gas = analogRead(MQ135_PIN);

  if (isnan(temperatura) || isnan(umidade)) {

    Serial.println("Erro ao ler DHT22");
    delay(2000);
    return;
  }

  Serial.println("------ DADOS ------");

  Serial.print("Temperatura: ");
  Serial.println(temperatura);

  Serial.print("Umidade: ");
  Serial.println(umidade);

  Serial.print("Gas: ");
  Serial.println(gas);

  // =========================
  // JSON MQTT
  // =========================

  String payload = "{";
  payload += "\"temperatura\":";
  payload += temperatura;
  payload += ",";
  payload += "\"umidade\":";
  payload += umidade;
  payload += ",";
  payload += "\"gas\":";
  payload += gas;
  payload += "}";

  client.publish("iot/sensores/dados", payload.c_str(), true);

  Serial.println("Dados enviados MQTT!");
  Serial.println(payload);

  // =========================
  // ALERTA
  // =========================

  if (gas > 3000 || temperatura > 60) {

    Serial.println("⚠ ALERTA AMBIENTAL!");

    digitalWrite(LED_PIN, HIGH);

    tone(BUZZER_PIN, 1000);

  } else {

    digitalWrite(LED_PIN, LOW);

    noTone(BUZZER_PIN);
  }

  Serial.println("--------------------");

  delay(2000);
}


## Como executar

1. Abrir o projeto no Wokwi
2. Instalar a biblioteca PubSubClient
3. Executar o código no ESP32
4. Conectar ao broker HiveMQ
5. Monitorar os dados MQTT em tempo real

## Simulação Wokwi

https://wokwi.com/projects/462494399325855745
