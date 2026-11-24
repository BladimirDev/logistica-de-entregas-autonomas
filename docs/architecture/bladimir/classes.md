# Diagrama de Classes e Interfaces - Despacho de Missão

As interfaces abaixo têm os métodos chamados no [diagrama de sequência](sequencia.md).

```mermaid
classDiagram
    direction LR

    class IAplicacaoWeb {
        <<interface>>
        +despacharEntrega(entregaId) void
        +notificarStatus(MissaoResponseDTO) void
    }
    class IDespachoService {
        <<interface>>
        +despachar(DespachoDTO) MissaoResponseDTO
        +onConfirmacao(ConfirmacaoDTO) void
    }
    class IPlanejadorRotas {
        <<interface>>
        +calcularRota(RotaRequestDTO) RotaDTO
    }
    class IMissaoRepository {
        <<interface>>
        +criarMissao(entregaId) MissaoDTO
        +buscarAgvsDisponiveis() List~AgvDTO~
        +reservarAgv(agvId, missaoId, versao) ReservaDTO
        +atualizarStatus(missaoId, status) void
    }
    class IBrokerMqtt {
        <<interface>>
        +publicarMissao(MissaoDTO) void
        +enviarParaRecarga(agvId) void
        +confirmarMissao(ConfirmacaoDTO) void
    }
    class IAgv {
        <<interface>>
        +receberMissao(MissaoDTO) void
    }

    class DespachoService {
        -planejador: IPlanejadorRotas
        -repositorio: IMissaoRepository
        -broker: IBrokerMqtt
        +despachar(DespachoDTO) MissaoResponseDTO
        +onConfirmacao(ConfirmacaoDTO) void
        -bateriaSuficiente(AgvDTO, RotaDTO) boolean
    }

    class MissaoAGV {
        -id: UUID
        -entregaId: UUID
        -agvId: UUID
        -status: StatusMissao
        -versao: int
        +podeCancelar() boolean
    }

    class StatusMissao {
        <<enumeration>>
        CRIADA
        AGUARDANDO_AGV
        DESPACHADA
        EM_EXECUCAO
        CONCLUIDA
        CANCELADA
        FALHA
    }

    IDespachoService <|.. DespachoService
    DespachoService --> IPlanejadorRotas
    DespachoService --> IMissaoRepository
    DespachoService --> IBrokerMqtt
    IMissaoRepository ..> MissaoAGV
    MissaoAGV --> StatusMissao
```

| Participante na sequência | Interface |
|---|---|
| Aplicação Web | `IAplicacaoWeb` |
| API de Orquestração | `IDespachoService` |
| Planejador de Rotas | `IPlanejadorRotas` |
| Banco de Dados | `IMissaoRepository` |
| Broker MQTT | `IBrokerMqtt` |
| AGV | `IAgv` |
