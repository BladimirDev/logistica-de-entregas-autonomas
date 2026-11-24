# Telemetria e Alertas dos AGVs

## Funcionalidade escolhida

Minha parte é a **telemetria dos AGVs**, ou seja, como o sistema recebe os dados que os veículos mandam e o que faz com eles.

Funciona assim:
1. o AGV manda posição, bateria e velocidade a cada 1 segundo pelo broker MQTT;
2. o Serviço de Telemetria recebe, confere se a mensagem está certa e, ao mesmo tempo, salva no banco e verifica a bateria;
3. se a bateria estiver abaixo de 15%, sai um alerta e o AGV fica como `BATERIA_BAIXA` (ele para de receber missões);
4. se estiver tudo normal, só atualiza a posição no mapa do operador;
5. se o AGV ficar mais de 5 segundos sem mandar nada, ele é marcado como `OFFLINE`.

Essa parte é importante porque o despacho e o replanejamento de rotas dependem de saber onde cada AGV está e quanta bateria ele tem.

## Arquivos

| Artefato | Arquivo |
|---|---|
| Diagrama de Classes e Interfaces | [classes.md](classes.md) |
| Diagrama de Sequência | [sequencia.md](sequencia.md) |
| Diagrama de Atividades (raias + fork/join) | [atividades.md](atividades.md) |
| Diagrama de Estados | [estados.md](estados.md) |
| C4 Nível 3 - Serviço de Telemetria | [c4-nivel3-telemetria.puml](c4-nivel3-telemetria.puml) |
| ADR 0002 - MQTT para a telemetria dos AGVs | [adr-0002-mqtt-telemetria.md](../../adr/adr-0002-mqtt-telemetria.md) |

Os diagramas `.md` usam Mermaid (o GitHub mostra direto). O C4 está em PlantUML, igual ao resto do repositório.
