# ADR 0002 - Usar MQTT para receber a telemetria dos AGVs

**Status:** Aprovado

## Contexto

Cada AGV manda a posição e a bateria uma vez por segundo. Dentro dos galpões o Wi-Fi às vezes cai por causa das estruturas de metal, então a conexão dos AGVs não é muito estável.

Precisávamos de uma forma leve de mandar esses dados, que gaste pouca bateria do AGV e que avise o sistema quando um AGV desconectar. Também queríamos poder mandar comandos de volta para o AGV.

Opções que a gente considerou:
- **HTTP (POST a cada segundo):** fácil de fazer, mas abre uma requisição nova toda hora e o servidor não consegue mandar nada para o AGV.
- **WebSocket direto na API:** funciona nos dois sentidos, mas a API teria que cuidar de todas as conexões dos AGVs.
- **MQTT:** protocolo feito para IoT, mensagens pequenas e conexão que fica aberta.

## Decisão

Usar **MQTT** com o broker **Mosquitto**. Cada AGV publica no tópico `agv/{id}/telemetria` com QoS 0, porque se perder uma leitura não tem problema, a próxima chega em 1 segundo. Para comandos e missões usamos QoS 1, que garante a entrega.

Também usamos o **Last Will** do MQTT: se o AGV cair sem avisar, o broker publica uma mensagem dizendo que ele desconectou.

Os alertas já processados saem do Serviço de Telemetria pelo **RabbitMQ** para a API, assim o MQTT fica só na comunicação com os AGVs.

## Consequências

- **Vantagens:** mensagens leves, menos gasto de bateria, detecção rápida de AGV desconectado e dá para mandar comandos para o AGV pelo mesmo canal.
- **Desvantagens:** é mais um serviço (o broker) para manter rodando. Com QoS 0 algumas leituras podem se perder. A equipe também precisa aprender MQTT e RabbitMQ.
