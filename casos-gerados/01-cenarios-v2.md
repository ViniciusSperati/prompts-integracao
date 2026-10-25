# Cenários — v2 refinada

Casos estruturados e revisados pelo ChatGPT nesta conversa em 25/10/2026. Contrato didático fornecido no mesmo pacote. Não são saídas brutas de sessões independentes nem resultados de execução automatizada.

Preparação comum: API de Pedidos real apontando para servidor HTTP simulado de Estoque; limpar o histórico de chamadas entre casos; enviar `Content-Type: application/json`. Para todas as chamadas recebidas pelo Estoque, verificar `POST /reservas`, Content-Type JSON e corpo semanticamente igual ao payload do pedido, sem depender da ordem das propriedades.

| ID | Fluxo | Preparação | Payload do pedido | Resposta esperada | Verificações da integração |
|---|---|---|---|---|---|
| C2-01 | Reserva bem-sucedida | Estoque responde 201 e `{"reservaId":"R001"}` | `{"produtoId":"P001","quantidade":2}` | 201 e `{"status":"CONFIRMADO","reservaId":"R001"}` | Uma chamada; reservaId propagado sem alteração |
| C2-02 | Quantidade mínima válida | Estoque responde 201 e `{"reservaId":"R002"}` | `{"produtoId":"P001","quantidade":1}` | 201 e `{"status":"CONFIRMADO","reservaId":"R002"}` | Uma chamada; quantidade 1 preservada |
| C2-03 | Estoque insuficiente | Estoque responde 409 | `{"produtoId":"P001","quantidade":2}` | 409; sem confirmação | Uma chamada; mapeamento de erro de negócio preservado |
| C2-04 | Produto inexistente | Estoque responde 404 | `{"produtoId":"INEXISTENTE","quantidade":1}` | 422; sem confirmação | Uma chamada; 404 da dependência traduzido para 422 |
| C2-05 | Quantidade zero — proteção da fronteira | Servidor simulado ativo, sem requisições anteriores | `{"produtoId":"P001","quantidade":0}` | 400 | Zero chamadas ao Estoque |
| C2-06 | ProdutoId ausente — proteção da fronteira | Servidor simulado ativo, sem requisições anteriores | `{"quantidade":1}` | 400 | Zero chamadas ao Estoque |

Nos casos de erro, “sem confirmação” verifica que a resposta não afirma status CONFIRMADO. A ausência de confirmação persistida exige um mecanismo de observação ainda não definido no contrato.
