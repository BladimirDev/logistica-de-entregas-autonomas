# Diagrama de Classes e Interfaces - Replanejamento de Rota

Métodos usados no [diagrama de sequência](sequencia.md).

```mermaid
classDiagram
    direction LR

    class IReplanejamentoService {
        <<interface>>
        +informarBloqueio(BloqueioDTO) void
        +onRotaConfirmada(ConfirmacaoDTO) void
    }
    class IRotaRepository {
        <<interface>>
        +buscarRotasNoSegmento(segmentoId) List~RotaDTO~
        +atualizarStatusRota(rotaId, status) void
        +salvarNovaRota(rotaId, NovaRotaDTO) void
    }
    class IPlanejadorRotas {
        <<interface>>
        +recalcularRota(RecalculoDTO) NovaRotaDTO
    }
    class IBrokerMqtt {
        <<interface>>
        +enviarParada(agvId) void
        +enviarNovaRota(agvId, NovaRotaDTO) void
        +confirmarRota(agvId) void
    }
    class IAgv {
        <<interface>>
        +parar() void
        +receberRota(NovaRotaDTO) void
    }
    class IAplicacaoWeb {
        <<interface>>
        +avisarReplanejamento(NovaRotaDTO) void
        +avisarOperador(AlertaDTO) void
    }

    class ReplanejamentoService {
        -repositorio: IRotaRepository
        -planejador: IPlanejadorRotas
        -broker: IBrokerMqtt
        +informarBloqueio(BloqueioDTO) void
        +onRotaConfirmada(ConfirmacaoDTO) void
        -rotaAceitavel(NovaRotaDTO) boolean
    }

    class Rota {
        -id: UUID
        -agvId: UUID
        -segmentos: List~String~
        -tentativas: int
        -status: StatusRota
    }

    class StatusRota {
        <<enumeration>>
        CALCULADA
        ATIVA
        RECALCULANDO
        AGUARDANDO_CONFIRMACAO
        CONCLUIDA
        INVALIDADA
    }

    IReplanejamentoService <|.. ReplanejamentoService
    ReplanejamentoService --> IRotaRepository
    ReplanejamentoService --> IPlanejadorRotas
    ReplanejamentoService --> IBrokerMqtt
    IRotaRepository ..> Rota
    Rota --> StatusRota
```

| Participante na sequência | Interface |
|---|---|
| API de Orquestração | `IReplanejamentoService` |
| Banco de Dados | `IRotaRepository` |
| Planejador de Rotas | `IPlanejadorRotas` |
| Broker MQTT | `IBrokerMqtt` |
| AGV | `IAgv` |
| Aplicação Web | `IAplicacaoWeb` |
