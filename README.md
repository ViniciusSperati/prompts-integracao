# Prompts para testes de integração — Pedidos e Estoque

Material para o fórum da Unidade III sobre uso de GenAI em testes de integração. O cenário é uma API de Pedidos que solicita reservas a uma API de Estoque.

**Estado:** documentação de prompts e casos propostos. Nenhuma API foi implementada e nenhum teste foi executado.

## Estrutura

| Pasta / arquivo | Conteúdo |
|---|---|
| [contrato-api.md](contrato-api.md) | Contrato didático, fronteira do teste, respostas e lacunas |
| [prompts/01-cenarios-v1-simples.md](prompts/01-cenarios-v1-simples.md) | Prompt simples para cenários funcionais |
| [prompts/01-cenarios-v2-refinado.md](prompts/01-cenarios-v2-refinado.md) | Prompt estruturado com contrato e verificações |
| [prompts/02-erros-v1-simples.md](prompts/02-erros-v1-simples.md) | Prompt simples para erros e dados |
| [prompts/02-erros-v2-refinado.md](prompts/02-erros-v2-refinado.md) | Prompt com exemplo, técnicas de teste e falhas de comunicação |
| [casos-gerados/01-cenarios-v1.md](casos-gerados/01-cenarios-v1.md) | Lista inicial ilustrativa de cenários |
| [casos-gerados/01-cenarios-v2.md](casos-gerados/01-cenarios-v2.md) | Cenários detalhados e revisados |
| [casos-gerados/02-erros-v1.md](casos-gerados/02-erros-v1.md) | Lista inicial ilustrativa de erros |
| [casos-gerados/02-erros-v2.md](casos-gerados/02-erros-v2.md) | Casos negativos detalhados e uma lacuna para investigação |
| [postagem-forum.md](postagem-forum.md) | Texto para o fórum, com o link deste repositório |

## Referência de organização

A organização segue o exemplo de [BrunoCasavechia/zombieplus-integration-prompts](https://github.com/BrunoCasavechia/zombieplus-integration-prompts), consultado em 25/10/2026, commit `4050c1650227d517c210788fc73083861b1c592e`: contrato separado, dois tipos de prompt em v1/v2 e arquivos de casos correspondentes.

O domínio e o conteúdo deste material foram elaborados para Pedidos e Estoque. Os resultados e o método experimental relatados no projeto de referência não são resultados deste trabalho.

## Como o material foi elaborado

1. Foi definido um contrato didático para a comunicação REST entre Pedidos e Estoque.
2. O ChatGPT apoiou a elaboração de prompts funcionais e de falhas, além dos casos e da revisão de aderência ao contrato.
3. O material foi organizado em versões simples e refinadas para evidenciar a diferença de especificidade das instruções.
4. A elaboração ocorreu nesta mesma conversa, com histórico disponível. As versões v1 são listas ilustrativas; os arquivos v2 são casos estruturados e revisados. Não se trata de quatro execuções independentes nem de preservação de saídas brutas.

## Comparação qualitativa dos materiais

| Aspecto | Cenários v1 | Cenários v2 | Erros v1 | Erros v2 |
|---|---|---|---|---|
| Quantidade de linhas de casos | 4 | 6 | 4 | 12, incluindo 1 investigação |
| Payload completo por caso | Não | Sim | Não | Sim |
| Preparação de dependência descrita | Não | Sim | Não | Sim |
| Verificação de chamadas | Não | Sim | Não | Sim, distinguindo conexão de requisição |
| Técnicas de teste explicitadas | Não | Não exigidas | Não | Sim |
| Lacuna identificada sem inventar resultado | Não abordada | Orientação no prompt | Não abordada | Sim: limite máximo de quantidade |

As contagens descrevem os arquivos deste pacote; não medem eficácia da LLM. O prompt funcional v2 pede seis casos e o prompt de erros v2 enumera a cobertura desejada. Portanto, o aumento de quantidade não é uma descoberta experimental. Há duas sobreposições entre as listas v2; não somar as linhas como testes únicos.

## O que cada tipo de prompt contribui

**Cenários funcionais:** orienta a revisão do fluxo de reserva e da tradução de respostas entre serviços. A versão detalhada especifica o que enviar, como preparar a dependência e o que observar na comunicação.

**Erros e dados de teste:** amplia o levantamento para classes de entrada inválida, indisponibilidade, timeout e quebra do contrato de resposta. O exemplo fornecido orienta o nível de detalhe. Identificar uma lacuna também é um resultado útil: evita transformar uma suposição em requisito.

Essa comparação é uma análise do material elaborado, não uma conclusão causal de experimento controlado sobre qual prompt é superior.

## Ajustes de prompt

1. Fornecer o contrato completo para restringir rotas, campos e códigos HTTP.
2. Definir a fronteira: API de Pedidos real e Estoque simulado por HTTP.
3. Pedir verificações da comunicação, além do status final.
4. Definir timeout e ausência de retentativas para evitar expectativas contraditórias.
5. Solicitar payloads concretos e formato tabular.
6. Marcar regras não definidas como investigação, sem inventar rollback ou persistência.
7. Distinguir uma tentativa de conexão de uma requisição recebida.

## Limites e próximos passos

- O contrato foi proposto para o exercício, não extraído de código existente.
- Casos que bloqueiam a chamada ao Estoque são proteção da fronteira e não exercitam comunicação externa.
- Um servidor simulado permite testar o cliente HTTP de Pedidos, mas não valida a implementação real de Estoque.
- Timeout não prova ausência de reserva; idempotência e reconciliação exigem definição adicional.
- Não há evidência de aprovação ou falha dos testes, nem validação de persistência.

Para uma comparação experimental adicional, executar cada prompt em sessão nova com contexto controlado, anexar o contrato somente às versões que o solicitam, preservar a saída bruta e registrar modelo, data e configuração disponíveis. Avaliar aderência ao contrato, cobertura da comunicação, verificabilidade e suposições. Repetições são necessárias para avaliar variabilidade. Não substituir estas descrições por alegações de rodadas não realizadas.

## Repositório e postagem

Repositório: [ViniciusSperati/prompts-integracao](https://github.com/ViniciusSperati/prompts-integracao).

O arquivo [postagem-forum.md](postagem-forum.md) contém o texto para revisar e publicar no fórum da disciplina.
