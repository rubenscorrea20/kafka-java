# Webhook Agnostico

## Pontos Importantes para o webhook

### Recursos para o webhook:

- no minimo uma entrega para cada evento, sem perda

- eventos em geral devem preservar a ordenação vinda dos produtores

- Devem conter numero sequencial dos eventos. O consumidor deve reter e garantir que os eventos sejam armazenados e totalmente processados antes da confirmação.

- Base de dados para armazenar os conteudos para envio das franquias

- Deve conter endpoint de subscription (Bacana talvez criar uma interface para gerenciar os endpoints, portal dev)

| action                   |  description                                                         |
| -------------------------| -------------------------------------------------------------------- |
|  GET /webhook/types      | List all available webhook types                                     |
|  GET /webhook            | List client's existing webhooks                                      |
|  GET /webhook/{id}       | Retrive details of a client's existing webhook                       |
|  POST /webhook           | Subscribe to a new webhook with details sent in request body         |
|  POST /webhook/validate  | Validate webhook with health check                                   |
|  PUT /webhook/{id}       | Update a client's existing webhook with details sent in request body |
|  DELETE /webhook/{id}    | Unsubscribe from a client's existing webhook                         |

- Event driven com camada de abstração para a ferramenta de pub/sub. Isso vai facilitar a produção de eventos para qualquer um que tenha que enviar eventos para franquia

- Retry policy para envio da franquia, qualquer codigo diferente de 2xx deve ser considerado como uma falha

- Evitar processos batch, envio de um evento por vez para a franquia, isso vai facilitar no retry policy

- Evitar dados sensíveis nos payloads para a franquia

### Segurança

- Assegurar o cadastro correto das URL de callback da franquia, sendo validada por um endpoint de validate

- usar HTTPS URLs

- Modelo de Autenticação e autorização.

- whitelist no lado das franquias para nossas chamadas.

- Resposta rapida ao recebimento do evento por parte da franquia, um 200 ok esta bom

- Unico event Id

### Scale

- Payload dos eventos deve ser bem leve

- Metricas do Kafka para scale dos pods (kafka exporter configurado para o MSK)

- Configuração minima de replication e partitions para os topicos do kafka

- Tracing
