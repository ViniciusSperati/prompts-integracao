# Prompt 1 — Cenários de integração · v2 refinada

Você é um engenheiro de qualidade especializado em integração de APIs REST.

Considere o contrato fornecido em `contrato-api.md`. Ao usar este prompt em outra conversa, anexe esse arquivo ou cole seu conteúdo integral antes da tarefa; o nome do arquivo, sozinho, não fornece seu conteúdo à LLM.

Gere seis casos funcionais para a integração entre Pedidos e Estoque. Cubra reserva bem-sucedida, quantidade mínima válida, estoque insuficiente, produto inexistente e duas situações de entrada inválida que devem impedir a chamada à dependência.

Na futura automação, use a API de Pedidos e seu cliente HTTP reais, conectados a um servidor simulado de Estoque. Não simule o método interno que faz a chamada HTTP.

Regras:
- Use apenas rotas e comportamentos definidos no contrato.
- Para entrada válida, verifique método, caminho, Content-Type, corpo enviado e quantidade de chamadas ao Estoque.
- Para entrada inválida, verifique que o servidor simulado não recebeu requisições. Classifique esse caso como proteção da fronteira, sem comunicação externa.
- Não invente persistência, rota de consulta, retentativa ou rollback.
- Marque qualquer comportamento não definido como NÃO ESPECIFICADO — investigar.
- Forneça payloads concretos e resultados verificáveis; não use alternativas como “400 ou 404”.

Formato: ID | Fluxo | Preparação | Payload do pedido | Resposta esperada | Verificações da integração.
