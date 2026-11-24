# Despacho de Missão com Checagem de Bateria

## Sobre a funcionalidade

Escolhi o **despacho de missão**. É quando o operador pede para uma entrega ser feita e o sistema precisa escolher qual AGV vai fazer ela.

O ponto principal é a bateria: o AGV só pode receber a missão se a bateria dele for maior ou igual ao consumo estimado da rota + 20% de margem. Se não tiver bateria, ele é mandado para recarregar e o sistema tenta o próximo AGV. Se nenhum servir, a missão fica aguardando.

Depois que a missão é enviada, o AGV tem 10 segundos para confirmar. Se não confirmar, o AGV é liberado e a missão volta a aguardar.

Escolhi essa parte porque se ela der errado o AGV pode ficar sem bateria no meio do caminho ou dois AGVs podem ser mandados para a mesma entrega.

## Arquivos

| Artefato | Arquivo |
|---|---|
| Diagrama de Classes e Interfaces | [classes.md](classes.md) |
| Diagrama de Sequência | [sequencia.md](sequencia.md) |
| Diagrama de Atividades (raias + fork/join) | [atividades.md](atividades.md) |
| Diagrama de Estados | [estados.md](estados.md) |
| C4 Nível 3 - API de Orquestração (despacho) | [c4-nivel3-api-despacho.puml](c4-nivel3-api-despacho.puml) |
| ADR 0001 - Bloqueio otimista para reservar o AGV | [adr-0001-bloqueio-otimista-reserva-agv.md](../../adr/adr-0001-bloqueio-otimista-reserva-agv.md) |

Os diagramas `.md` usam Mermaid (o GitHub mostra direto). O C4 está em PlantUML, igual ao resto do repositório.
