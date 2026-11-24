# Diagrama de Classes e Interfaces - Telemetria

Métodos usados no [diagrama de sequência](sequencia.md).

```mermaid
classDiagram
    direction LR

    class IBrokerMqtt {
        <<interface>>
        +publicarTelemetria(TelemetriaDTO) void
    }
    class ITelemetriaService {
        <<interface>>
        +onTelemetria(TelemetriaDTO) void
        +verificarSemSinal(agvId) void
    }
    class ILeituraRepository {
        <<interface>>
        +salvarLeitura(TelemetriaDTO) void
        +atualizarStatusAgv(agvId, status) AgvDTO
    }
    class IFilaEventos {
        <<interface>>
        +publicarAlerta(AlertaDTO) void
        +publicarPosicao(PosicaoDTO) void
    }
    class IAlertaHandler {
        <<interface>>
        +onAlerta(AlertaDTO) void
        +onPosicao(PosicaoDTO) void
    }
    class IAplicacaoWeb {
        <<interface>>
        +mostrarAlerta(AlertaDTO) void
        +atualizarMapa(PosicaoDTO) void
    }

    class TelemetriaService {
        -repositorio: ILeituraRepository
        -fila: IFilaEventos
        -ultimoSinal: Map~UUID, DateTime~
        +onTelemetria(TelemetriaDTO) void
        +verificarSemSinal(agvId) void
        -bateriaBaixa(TelemetriaDTO) boolean
    }

    class VeiculoAGV {
        -id: UUID
        -bateria: int
        -status: StatusAGV
        -ultimaLeitura: DateTime
    }

    class StatusAGV {
        <<enumeration>>
        LIGANDO
        DISPONIVEL
        EM_MISSAO
        BATERIA_BAIXA
        CARREGANDO
        OFFLINE
        MANUTENCAO
        DESATIVADO
    }

    ITelemetriaService <|.. TelemetriaService
    TelemetriaService --> ILeituraRepository
    TelemetriaService --> IFilaEventos
    ILeituraRepository ..> VeiculoAGV
    VeiculoAGV --> StatusAGV
```

| Participante na sequência | Interface |
|---|---|
| Broker MQTT | `IBrokerMqtt` |
| Serviço de Telemetria | `ITelemetriaService` |
| Banco de Dados | `ILeituraRepository` |
| RabbitMQ | `IFilaEventos` |
| API de Orquestração | `IAlertaHandler` |
| Aplicação Web | `IAplicacaoWeb` |
