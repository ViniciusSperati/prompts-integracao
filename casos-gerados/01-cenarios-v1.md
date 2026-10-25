# Cenários — v1 simples

Material didático elaborado pelo ChatGPT nesta conversa em 25/10/2026, com histórico disponível. Lista inicial ilustrativa, não saída bruta de uma execução isolada do prompt. Nenhum teste foi executado.

| ID | Cenário | Resultado em nível de intenção |
|---|---|---|
| C1-01 | Criar pedido com produto disponível | Pedido confirmado e reserva associada |
| C1-02 | Criar pedido sem estoque suficiente | Pedido não confirmado e erro de negócio retornado |
| C1-03 | Informar produto inexistente | Erro retornado ao cliente |
| C1-04 | Estoque indisponível | Falha tratada sem confirmação do pedido |

## Revisão

A lista identifica intenções relevantes, mas não especifica payloads, preparação, status HTTP nem verificação da requisição entre componentes. Essas ausências dificultam transformar os itens em testes reproduzíveis. Não foram introduzidos erros propositais para favorecer a versão refinada.
