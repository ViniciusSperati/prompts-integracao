# Contrato didático — Pedidos e Estoque

Contrato definido para este exercício, sem correspondência com uma aplicação real inspecionada. As regras abaixo são requisitos propostos, não comportamentos comprovados por execução.

## Componentes e fronteira do teste

Cliente HTTP → API de Pedidos → cliente HTTP de Estoque → servidor simulado de Estoque.

Na futura automação, a API de Pedidos e seu cliente HTTP devem ser reais. Somente a dependência Estoque será simulada por um servidor HTTP. Isso permite verificar serialização, comunicação e tratamento das respostas, mas não comprova compatibilidade com a implementação real de Estoque nem constitui teste ponta a ponta.

## Entrada e chamada de saída

- Cliente envia `POST /pedidos`, com `Content-Type: application/json`.
- Payload válido: `{"produtoId":"P001","quantidade":2}`.
- `produtoId`: string obrigatória, não vazia e não composta apenas por espaços.
- `quantidade`: número inteiro obrigatório e maior que zero; strings numéricas não são aceitas.
- Entrada inválida retorna HTTP 400 e não provoca chamada ao Estoque.
- Entrada válida provoca um `POST /reservas`, com `Content-Type: application/json` e os mesmos valores de `produtoId` e `quantidade`.
- Não há retentativas automáticas. O orçamento de espera pela resposta do Estoque é de 2 segundos. Na automação, medir esse intervalo na chamada de saída; o tempo total de `/pedidos` inclui processamento adicional.

## Respostas

| Resposta ou falha do Estoque | Resposta da API de Pedidos |
|---|---|
| 201 com JSON `{"reservaId":"R001"}` | 201 com JSON `{"status":"CONFIRMADO","reservaId":"R001"}` |
| 409 — estoque insuficiente | 409; pedido não confirmado |
| 404 — produto inexistente | 422; pedido não confirmado |
| 500 | 503; pedido não confirmado |
| Conexão recusada | 503; pedido não confirmado |
| Ausência de resposta dentro do orçamento de espera | 504; pedido não confirmado |
| 201 com JSON malformado | 502; pedido não confirmado |
| 201 com JSON sem `reservaId` | 502; pedido não confirmado |

O corpo das respostas de erro não foi definido. Não exigir campos ou mensagens específicos. A resposta de erro não pode afirmar `status: CONFIRMADO`.

## Lacunas deliberadas

Não estão definidos: autenticação, limite máximo de quantidade, tamanho máximo de produtoId, tratamento de campos extras, tipos alternativos de reservaId, demais status do Estoque, idempotência, persistência, compensação e reconciliação. Casos dependentes dessas regras devem ser marcados como **NÃO ESPECIFICADO — investigar**.

Não existem rotas de consulta ou cancelamento definidas neste contrato. Não inventá-las para verificar efeitos colaterais. Sem um mecanismo de consulta ao estado, verificar a resposta HTTP não comprova ausência de persistência interna.

Um timeout não prova que o Estoque deixou de criar uma reserva. Não presumir rollback, liberação ou cancelamento automático.
