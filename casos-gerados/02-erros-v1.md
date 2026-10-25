# Erros e dados de teste — v1 simples

Material didático elaborado pelo ChatGPT nesta conversa em 25/10/2026, com histórico disponível. Lista inicial ilustrativa, não saída bruta de uma execução isolada. Nenhum teste foi executado.

| ID | Cenário | Dado ou condição | Resultado em nível de intenção |
|---|---|---|---|
| E1-01 | Quantidade inválida | Quantidade negativa | Rejeitar a entrada |
| E1-02 | Produto não informado | Ausência do produto | Rejeitar a entrada |
| E1-03 | Estoque retorna erro | Erro no serviço dependente | Retornar erro tratado |
| E1-04 | Estoque demora a responder | Resposta lenta | Encerrar a espera conforme timeout |

## Revisão

Os itens não fixam payloads completos, falhas específicas, orçamento de timeout, códigos esperados nem chamadas permitidas. “Erro no serviço” pode significar HTTP 500, conexão recusada ou resposta inválida, situações distintas que precisam de preparações diferentes.
