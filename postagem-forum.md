# Texto para o fórum

Para esta atividade, escolhi a integração entre uma API de pedidos e uma API de estoque. A API de pedidos solicita uma reserva e utiliza a resposta da dependência para confirmar a operação ou retornar um erro.

Organizei o material em contrato, prompts e casos gerados. Trabalhei com dois tipos de prompt: geração de cenários funcionais de integração e geração de casos de erro e dados de teste. Para cada tipo, documentei uma versão simples e outra refinada.

No prompt funcional refinado, forneci o contrato e solicitei payloads, preparação da dependência e verificações de método, caminho, corpo e quantidade de chamadas. O resultado ficou mais específico e verificável do que a lista inicial de intenções: além de verificar o status retornado, os casos descrevem como conferir os dados enviados entre os serviços.

No prompt de erros, incluí um exemplo de detalhamento e pedi técnicas de teste conforme o cenário. Isso direcionou a elaboração para quantidades inválidas, conexão recusada, HTTP 500, timeout e respostas incompatíveis com o contrato. A diversidade está nas condições de integração verificadas, não apenas na quantidade de linhas.

Os ajustes mais úteis foram explicitar o contrato, definir o timeout e a ausência de retentativas, solicitar dados concretos e marcar comportamentos não definidos como “NÃO ESPECIFICADO — investigar”. Também diferenciei uma tentativa de conexão de uma requisição recebida pelo estoque.

Um ponto relevante foi que timeout não garante que a reserva deixou de acontecer. Exigir rollback ou cancelamento nesse caso seria inventar um comportamento. Idempotência e reconciliação precisam ser definidas antes de gerar testes com essas expectativas.

Repositório: [ViniciusSperati/prompts-integracao](https://github.com/ViniciusSperati/prompts-integracao)

O material foi elaborado com apoio do ChatGPT nesta conversa, com histórico disponível. As versões simples são ilustrativas e os casos refinados foram estruturados e revisados; não representam quatro execuções independentes nem testes automatizados já executados. Essa limitação impede atribuir as diferenças apenas ao tipo de prompt.

A GenAI contribuiu como copiloto na elaboração dos cenários e na identificação de lacunas. A revisão do contrato, a definição dos critérios de aprovação e a execução dos testes continuam necessárias para validar a suíte.
