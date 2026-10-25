# Erros e dados de teste — v2 refinada

Casos estruturados e revisados pelo ChatGPT nesta conversa em 25/10/2026. Não são saídas brutas de sessões independentes. Nenhum teste foi executado.

Preparação comum: enviar `POST /pedidos` com Content-Type JSON; limpar o histórico do servidor simulado a cada caso. Nas chamadas recebidas, verificar `POST /reservas`, Content-Type JSON e preservação dos dados. PE = partição de equivalência; AVL = análise de valor limite.

| ID | Técnica e cenário | Payload | Preparação | Resultado e verificação | Fonte |
|---|---|---|---|---|---|
| E2-01 | AVL — imediatamente abaixo do mínimo | `{"produtoId":"P001","quantidade":0}` | Servidor simulado ativo | 400; zero chamadas; proteção da fronteira | Contrato |
| E2-02 | PE — quantidade negativa | `{"produtoId":"P001","quantidade":-1}` | Servidor simulado ativo | 400; zero chamadas; proteção da fronteira | Contrato |
| E2-03 | PE — quantidade fracionária | `{"produtoId":"P001","quantidade":1.5}` | Servidor simulado ativo | 400; zero chamadas; proteção da fronteira | Contrato |
| E2-04 | PE — tipo string | `{"produtoId":"P001","quantidade":"2"}` | Servidor simulado ativo | 400; zero chamadas; proteção da fronteira | Contrato |
| E2-05 | PE — campo obrigatório ausente | `{"quantidade":1}` | Servidor simulado ativo | 400; zero chamadas; proteção da fronteira | Contrato |
| E2-06 | PE — produto só com espaços | `{"produtoId":"   ","quantidade":1}` | Servidor simulado ativo | 400; zero chamadas; proteção da fronteira | Contrato |
| E2-07 | Injeção de falha — HTTP 500 | `{"produtoId":"P001","quantidade":2}` | Estoque recebe a requisição e retorna 500 | Pedidos retorna 503; uma chamada; sem retentativa nem confirmação | Contrato |
| E2-08 | Injeção de falha — conexão recusada | `{"produtoId":"P001","quantidade":2}` | Apontar Estoque para porta local controlada sem listener | 503; nenhuma requisição recebida; contar uma tentativa apenas com instrumentação do cliente ou rede | Contrato |
| E2-09 | Injeção de falha — timeout | `{"produtoId":"P001","quantidade":2}` | Estoque recebe a requisição e atrasa resposta em 3 s | 504; uma chamada; sem retentativa; orçamento de espera de 2 s; não inferir cancelamento da reserva | Contrato |
| E2-10 | Violação do contrato — JSON malformado | `{"produtoId":"P001","quantidade":2}` | Estoque retorna 201, Content-Type JSON e corpo literal `{"reservaId":` | 502; uma chamada; sem confirmação | Contrato |
| E2-11 | Violação do contrato — campo ausente | `{"produtoId":"P001","quantidade":2}` | Estoque retorna 201, Content-Type JSON e corpo `{}` | 502; uma chamada; sem confirmação | Contrato |
| E2-12 | Investigação — possível limite superior | `{"produtoId":"P001","quantidade":1000000}` | Definir primeiro se existe limite de quantidade | NÃO ESPECIFICADO — investigar; não exigir rejeição nem sucesso por um limite inventado | Lacuna |

## Observações para futura automação

- E2-12 é um item de investigação, não um teste com critério de aprovação definido. O valor 1000000 é uma sondagem e não um limite conhecido.
- E2-01 e E2-05 se sobrepõem a C2-05 e C2-06; deduplicar na suíte executável.
- A confirmação persistida do pedido não é observável por este contrato. Nos erros, verificar o status HTTP e a ausência de status CONFIRMADO na resposta; não afirmar que o banco foi inspecionado.
- No timeout, definir tolerância de medição ao implementar o teste; não usar comparação exata do tempo de parede.
