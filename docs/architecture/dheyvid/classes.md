# Diagrama de Classes e Interfaces - Ordens de Entrega

Métodos usados no [diagrama de sequência](sequencia.md).

```mermaid
classDiagram
    direction LR

    class IAplicacaoWeb {
        <<interface>>
        +preencherOrdem(dados) void
    }
    class IOrdemService {
        <<interface>>
        +criarOrdem(NovaOrdemDTO) OrdemResponseDTO
        +calcularPrioridade(OrdemDTO) int
    }
    class IGeolocalizacaoAPI {
        <<interface>>
        +validarPontos(origem, destino) PontosDTO
    }
    class IWmsAPI {
        <<interface>>
        +consultarEstoque(materialId, galpaoId) EstoqueDTO
        +reservarMaterial(ReservaDTO) ReservaDTO
    }
    class IOrdemRepository {
        <<interface>>
        +salvarOrdem(NovaOrdemDTO) OrdemDTO
        +atualizarOrdem(ordemId, status, prioridade) OrdemDTO
    }
    class IFilaOrdens {
        <<interface>>
        +publicarOrdem(OrdemDTO, prioridade) void
    }

    class OrdemService {
        -geo: IGeolocalizacaoAPI
        -wms: IWmsAPI
        -repositorio: IOrdemRepository
        -fila: IFilaOrdens
        +criarOrdem(NovaOrdemDTO) OrdemResponseDTO
        +calcularPrioridade(OrdemDTO) int
    }

    class OrdemEntrega {
        -id: UUID
        -origem: String
        -destino: String
        -materialId: UUID
        -quantidade: int
        -prazo: DateTime
        -prioridade: int
        -status: StatusOrdem
    }

    class StatusOrdem {
        <<enumeration>>
        SOLICITADA
        EM_VALIDACAO
        REJEITADA
        NA_FILA
        PLANEJADA
        EM_ANDAMENTO
        ENTREGUE
        CANCELADA
    }

    IOrdemService <|.. OrdemService
    OrdemService --> IGeolocalizacaoAPI
    OrdemService --> IWmsAPI
    OrdemService --> IOrdemRepository
    OrdemService --> IFilaOrdens
    IOrdemRepository ..> OrdemEntrega
    OrdemEntrega --> StatusOrdem
```

| Participante na sequência | Interface |
|---|---|
| Aplicação Web | `IAplicacaoWeb` |
| API de Orquestração | `IOrdemService` |
| API de Geolocalização | `IGeolocalizacaoAPI` |
| API de Armazéns (WMS) | `IWmsAPI` |
| Banco de Dados | `IOrdemRepository` |
| RabbitMQ | `IFilaOrdens` |
