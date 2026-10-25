# Prompt 2 — Erros e dados de teste · v2 refinada

Atue como engenheiro de qualidade. Considere o conteúdo integral de `contrato-api.md`, que deve ser anexado ou colado junto deste prompt.

Gere casos negativos para a integração Pedidos → Estoque, usando partição de equivalência, análise de valor limite, injeção de falhas e verificação de contrato, conforme aplicável.

Cubra:
1. Quantidade zero, negativa, fracionária e string numérica, em casos separados.
2. ProdutoId ausente e composto apenas por espaços.
3. HTTP 500, conexão recusada, atraso de 3 segundos, JSON malformado e ausência de reservaId.
4. Quantidade acima de um possível limite máximo, como lacuna do contrato.

Exemplo do nível de detalhe esperado:
Entrada: POST /pedidos com {"produtoId":"P001","quantidade":2}.
Preparação: servidor simulado recebe POST /reservas e responde HTTP 500.
Resultado: Pedidos retorna HTTP 503, sem confirmar o pedido.
Verificações: exatamente uma chamada, corpo preservado e nenhuma retentativa.

Regras:
- Cada linha deve conter dados concretos, preparação e resultado esperado.
- Entrada inválida não deve chamar Estoque; identifique esse caso como proteção da fronteira.
- Conexão recusada não é uma requisição recebida: para contar tentativas, é necessária instrumentação do cliente HTTP ou de rede.
- No timeout, use atraso de 3 segundos para ultrapassar o orçamento de 2 segundos. Não exija igualdade exata do tempo total de resposta; a futura automação deverá definir tolerância e relógio de medição.
- Não afirme que timeout significa ausência de reserva ou que houve rollback.
- Quando faltar uma regra, marque NÃO ESPECIFICADO — investigar, sem inventar status HTTP.

Formato: ID | Técnica e cenário | Payload | Preparação | Resultado e verificação | Fonte.
