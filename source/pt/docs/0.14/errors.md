title: Erros
---
Moleculer possui um manipulador `errors` integrado para gerar um erro nos serviços.

## Classes de erros base

### `MoleculerError`
A classe base de erros.

**Parâmetros**

| Nome      | Tipo     | Valor padrão | Descrição               |
| --------- | -------- | ------------ | ----------------------- |
| `message` | `String` |              | Mensagem de erro        |
| `code`    | `Number` | `500`        | Código do erro          |
| `type`    | `String` |              | Tipo de erro            |
| `data`    | `any`    |              | Qualquer dado relevante |

**Exemplo**
```js
const { MoleculerError } = require("moleculer").Errors;

throw new MoleculerError("Something happened", 501, "ERR_SOMETHING", { a: 5, nodeID: "node-666" });
```

### `MoleculerRetryableError`
Retornos para erros resilientes. É usado em `broker.call`. O broker tenta novamente requisições que foram rejeitadas por um erro `MoleculerRetryableError`.

**Parâmetros**

| Nome      | Tipo     | Valor padrão | Descrição               |
| --------- | -------- | ------------ | ----------------------- |
| `message` | `String` |              | Mensagem de erro        |
| `code`    | `Number` | `500`        | Código do erro          |
| `type`    | `String` |              | Tipo de erro            |
| `data`    | `any`    |              | Qualquer dado relevante |

**Exemplo**
```js
const { MoleculerRetryableError } = require("moleculer").Errors;

throw new MoleculerRetryableError("Some retryable thing happened", 501, "ERR_SOMETHING", { a: 5, nodeID: "node-666" });
```

### `MoleculerServerError`
Retornos para erros de servidor resilientes. Os parâmetros são os mesmos que `MoleculerRetryableError`.


### `MoleculerClientError`
Retornos para o cliente de erros que são **não** repetidos. Os parâmetros são os mesmos que `MoleculerError`.

## Classes de erro internas

### `ServiceNotFoundError`
Retorna se você fizer um `call` em uma ação de serviço não registrado. Código de erro: **404** Retryable: **true** Type: `SERVICE_NOT_FOUND`

### `ServiceNotAvailableError`
Retorna se você fizer um `call` em uma ação indisponível no momento. Ex.: nó desconectado que contém este serviço ou circuit breaker está aberto. Error code: **404** Retryable: **true** Type: `SERVICE_NOT_AVAILABLE`


### `RequestTimeoutError`
Retorna se a sua requisição atingiu o timeout. Error code: **504** Retryable: **true** Type: `REQUEST_TIMEOUT`

### `RequestSkippedError`
Retorna se sua chamada aninhada for ignorada porque a execução atingiu o timeout distribuído. Error code: **514** Retryable: **false** Type: `REQUEST_SKIPPED`

### `RequestRejectedError`
Retorna se o nó chamado for desconectado durante a requisição. Error code: **503** Retryable: **true** Type: `REQUEST_REJECTED`

### `QueueIsFullError`
Retorna se houver muitas solicitações ativas. Error code: **429** Retryable: **true** Type: `QUEUE_FULL`

### `ValidationError`
O validador retorna se os parâmetros de entrada não forem válidos. Error code: **422** Retryable: **false** Type: `VALIDATION_ERROR` _(default)_

### `MaxCallLevelError`
Retorna caso suas chamadas aninhadas tenham atingido o valor `maxCallLevel` (para evitar laços de chamada infinitos). Error code: **500** Retryable: **false** Type: `MAX_CALL_LEVEL`

### `ServiceSchemaError`
Retorna se o esquema de serviço não for válido. Error code: **500** Retryable: **false** Type: `SERVICE_SCHEMA_ERROR`

### `BrokerOptionsError`
Retorna se suas opções do broker não forem válidas. Error code: **500** Retryable: **false** Type: `BROKER_OPTIONS_ERROR`

### `GracefulStopTimeoutError`
Retorna se o desligamento atingir o timeout. Error code: **500** Retryable: **false** Type: `GRACEFUL_STOP_TIMEOUT`

### `ProtocolVersionMismatchError`
Retorna se um nodeID antigo estiver conectado com a versão do protocolo antigo. Error code: **500** Retryable: **false** Type: `PROTOCOL_VERSION_MISMATCH`

### `InvalidPacketDataError`
Retorne caso o módulo de transporte receba dados desconhecidos. Error code: **500** Retryable: **false** Type: `INVALID_PACKET_DATA`

## Criar erros personalizados
O exemplo a seguir mostra como criar uma classe personalizada de `erro` que é herdada de `MoleculerError`.

```js
const { MoleculerError } = require("moleculer").Errors;

class MyBusinessError extends MoleculerError {
    constructor(msg, data) {
        super(msg || `This is my business error.`, 500, "MY_BUSINESS_ERROR", data);
    }
}
```