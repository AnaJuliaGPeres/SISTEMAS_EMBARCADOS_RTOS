# Atividade de Sistemas Embarcados:


## Atividade realizada em sala de aula, instalando o modelo do nodemcu no proteus e tambem realizando as prototipações. Segue print da ativiade realizada validando o projeto no modelo esquemático, 3d e PCB:

# ---Modelo esquemático: 

![image](https://github.com/AnaJuliaGPeres/SISTEMAS_EMBARCADOS_RTOS/assets/126435035/6a571f28-e230-44b7-babe-cfcf37b068b1)



# ---Modelo PCB: 

![image](https://github.com/AnaJuliaGPeres/SISTEMAS_EMBARCADOS_RTOS/assets/126435035/69ef5af6-609d-43b9-9d98-2d30e4eb3a3c)


#  ---Modelo 3D: 

![image](https://github.com/AnaJuliaGPeres/SISTEMAS_EMBARCADOS_RTOS/assets/126435035/efe47259-a5da-49eb-b50d-1e7ea3ba2439)




#  ----Como funciona o MQTT: 

O MQTT  é um protocolo de comunicação leve, projetado para dispositivos de baixa largura de banda, alta latência ou ambientes de rede não confiáveis. Ele é amplamente utilizado em aplicações de Internet das Coisas (IoT) devido à sua eficiência e baixo consumo de recursos.

### Arquitetura e Componentes
Broker: É o servidor central que recebe todas as mensagens de clientes e as encaminha para os clientes subscritos aos tópicos relevantes.
Clientes: Dispositivos ou aplicações que publicam (enviam) e/ou subscrevem (recebem) mensagens. Os clientes podem ser sensores, atuadores, dispositivos móveis, etc.
Principais Conceitos
Publicação/Assinatura (Publish/Subscribe): O MQTT utiliza um modelo de comunicação onde os clientes podem publicar mensagens em tópicos específicos e assinar esses tópicos para receber mensagens.

###  Funcionamento do MQTT
Conexão: Um cliente se conecta ao broker usando um protocolo TCP/IP. Ele pode fornecer um nome de usuário e senha para autenticação e pode especificar se deseja uma sessão limpa;
Publicação: Um cliente publica uma mensagem em um tópico específico. A mensagem é enviada ao broker, que a encaminha para todos os clientes que assinaram esse tópico.
Assinatura: Um cliente assina um ou mais tópicos para receber mensagens. Ele pode usar curingas para se inscrever em vários tópicos de uma vez (ex: "casa/+/temperatura" ou "casa/#").

```  #include <Arduino.h>
#include <ESP8266WiFi.h>
#include <PubSubClient.h>

const char* ssid = "AndroidAP ";
const char* password = "nssa9988";
const char* mqtt_server = "192.168.14.69";

WiFiClient espClient;
PubSubClient client(espClient);
const int Relay = D1;

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] ");
  String message;
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println(message);

  if (String(topic) == "ControleRelay"){
    if (message == "ON") {
      digitalWrite(Relay, LOW);
    } else if (message == "OFF") {
      digitalWrite(Relay, HIGH);
    }
  }
}

void setup() {
  pinMode(Relay, OUTPUT);
  Serial.begin(9600);
  setup_wifi();
  client.setServer(mqtt_server, 1883);
  client.setCallback(callback);
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Attempting MQTT connection...");
    if (client.connect("ESP8266Client")) {
      Serial.println("connected");
      client.subscribe("ControleRelay");
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      delay(5000);
    }
  }
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}
 ´´´


### Pritns da Atividade:

### --- BROKER

![image](https://github.com/AnaJuliaGPeres/SISTEMAS_EMBARCADOS_RTOS/assets/126435035/5969d666-55f4-481d-8842-6a3dc6a16518)


### ---SUBSCRIBE

![image](https://github.com/AnaJuliaGPeres/SISTEMAS_EMBARCADOS_RTOS/assets/126435035/856ca7a6-f5d4-4341-8a9f-e081b1aa39e3)

### --- PUBLISH

![image](https://github.com/AnaJuliaGPeres/SISTEMAS_EMBARCADOS_RTOS/assets/126435035/b4ab7c09-f4aa-494a-9fe7-9c0d0a009fd8)

### RECEBENDO A MENSAGEM NO VSCODE:

![image](https://github.com/AnaJuliaGPeres/SISTEMAS_EMBARCADOS_RTOS/assets/126435035/4b5345f5-13a6-4874-8b73-af8f6af5090b)





