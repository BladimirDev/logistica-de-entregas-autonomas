# Cadastro e Priorização de Ordens de Entrega

## O que é a funcionalidade

Escolhi o **cadastro de ordens de entrega**, que é por onde toda entrega começa no sistema.

O operador informa de onde sai o material, para onde vai, qual material, a quantidade e o prazo. Antes de aceitar a ordem, o sistema confere duas coisas ao mesmo tempo:
- se a origem e o destino existem (API de Geolocalização);
- se tem material suficiente no galpão (API de Armazéns / WMS).

Se estiver tudo certo, o material é reservado, o sistema calcula a prioridade da ordem e coloca ela numa fila do RabbitMQ. Se não, a ordem é rejeitada e o operador vê o motivo.

A prioridade leva em conta a urgência e o prazo. Se uma ordem ficar muito tempo esperando, a prioridade dela aumenta.

## Arquivos

| Artefato | Arquivo |
|---|---|
| Diagrama de Classes e Interfaces | [classes.md](classes.md) |
| Diagrama de Sequência | [sequencia.md](sequencia.md) |
| Diagrama de Atividades (raias + fork/join) | [atividades.md](atividades.md) |
| Diagrama de Estados | [estados.md](estados.md) |
| C4 Nível 2 - Containers (versão atualizada do sistema todo) | [c4-nivel2-containers.puml](../c4-nivel2-containers.puml) |
| ADR 0003 - Fila com prioridade no RabbitMQ | [adr-0003-fila-prioridade-rabbitmq.md](../../adr/adr-0003-fila-prioridade-rabbitmq.md) |

Os diagramas `.md` usam Mermaid (o GitHub mostra direto). O C4 está em PlantUML, igual ao resto do repositório.
