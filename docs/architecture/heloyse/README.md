# Replanejamento de Rota por Bloqueio

## A funcionalidade

A minha parte é o **replanejamento de rota**. Às vezes um corredor do parque fica bloqueado (uma empilhadeira parada, uma área interditada) enquanto um AGV está no meio da entrega. A API de Tráfego avisa o sistema e ele precisa:

1. descobrir quais rotas passam pelo trecho bloqueado;
2. mandar o AGV parar em um lugar seguro e, ao mesmo tempo, pedir uma rota nova para o Planejador de Rotas;
3. se a rota nova atrasar no máximo 15 minutos, enviar para o AGV e esperar ele confirmar;
4. se não tiver rota boa, ou o AGV não confirmar depois de 3 tentativas, a rota é invalidada e o operador é avisado.

Se o bloqueio for liberado no meio do processo, o AGV volta para a rota original.

## Arquivos

| Artefato | Arquivo |
|---|---|
| Diagrama de Classes e Interfaces | [classes.md](classes.md) |
| Diagrama de Sequência | [sequencia.md](sequencia.md) |
| Diagrama de Atividades (raias + fork/join) | [atividades.md](atividades.md) |
| Diagrama de Estados | [estados.md](estados.md) |
| C4 Nível 3 - Planejador de Rotas | [c4-nivel3-planejador.puml](c4-nivel3-planejador.puml) |
| ADR 0004 - gRPC entre a API e o Planejador | [adr-0004-grpc-planejador.md](../../adr/adr-0004-grpc-planejador.md) |

Os diagramas `.md` usam Mermaid (o GitHub mostra direto). O C4 está em PlantUML, igual ao resto do repositório.
