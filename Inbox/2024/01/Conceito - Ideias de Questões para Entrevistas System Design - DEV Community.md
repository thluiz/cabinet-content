---
created: 2023-11-14T14:40:23 (UTC -03:00)
tags: [software,coding,development,engineering,inclusive,community]
source: https://dev.to/zanfranceschi/conceito-ideias-de-questoes-para-entrevistas-system-design-116m
author: Francisco Zanfranceschi
---

# [Conceito] - Ideias de Questões para Entrevistas: System Design - DEV Community

> ## Excerpt
> Conteúdo original em https://twitter.com/zanfranceschi/status/1569375318091386885      Ei dev,  Já...

---
> Conteúdo original em [https://twitter.com/zanfranceschi/status/1569375318091386885](https://twitter.com/zanfranceschi/status/1569375318091386885)

___

Ei dev,

Já fiz muitas entrevistas técnicas na condição de entrevistador e gosto particularmente de questões de System Design (ou Arquitetura – como queira chamar).

Nessa thread, coloco algumas questões que costumava fazer.

cc @sseraphini

↓

[![Image](https://res.cloudinary.com/practicaldev/image/fetch/s--riF77a46--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nt85vngi7uqvd89hog49.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--riF77a46--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nt85vngi7uqvd89hog49.png)

___

Antes de irmos para as questões, gostaria de compartilhar uma prática que me ajudava nas avaliações. Para perguntas abertas (que não têm uma resposta correta), costumava incluir a expectativa de resposta. Farei isso aqui também.

Legenda:  
Q – Questão  
ER – Expectativa de Resposta

___

Q: Quais foram/são suas atividades frequentes (aquelas que geram experiência prática) mais relevantes relacionadas à posição?

ER: Atividades similares ou que agregam valor à posição trabalhada.

___

Q: Quando acha adequado usar um banco NoSQL vs um banco relacional tradicional?

ER: Demonstrar conhecimentos sobre as diferenças entre os dois. Ex.: escalabilidade horizontal/vertical, ACID (alguns nosql oferecem ACID), complexidade de queries, padrões de acesso, etc.

___

Q: Você concorda que, numa aplicação de borda, com alta variabilidade de concorrência de acessos, usar um banco nosql é adequado? Por que?

ER: Demonstrar um pouco mais de conhecimento em NoSQL. Geralmente é adequado por causa da escalabilidade horizontal natural.

___

Q: Como escalar um banco relacional tradicional?

ER: No geral, verticalmente (mais/menos memória, CPU, storage, etc.).

obs.: Por incrível que pareça, muita gente responde "escalando horizontalmente" sem mencionar réplicas de leitura.

___

Q: Supondo dois tipos de load balancers – um que atua na camada TCP 4 e outro na camada 7 – em quais cenários você usaria um e outro?

ER: Basicamente um ser ignorante em relação ao conteúdo e o outro ser capaz de inspecionar headers, paths, etc; ideal para HTTP, por exemplo.

___

Q: Sobre os códigos de status do HTTP, você poderia me falar o que cada faixa representa (200, 300, 400, 500)?

ER: 200 resposta positiva do servidor / 300 redirecionamentos, cache / 400 erro do cliente / 500 erro do servidor.

___

Q: Como você aborda escalabilidade?

ER: Essa questão é muito aberta e boa para entender um pouco a maturidade da pessoa. Geralmente abordar async/sync, vertical/horizontal, cache, detectar over-engineering na resposta, etc.

___

Q: Como você lida com tolerância a falhas?

ER: Questão bem aberta também. Eliminar/diminuir pontos únicos de falhas, recuperação, monitoramento, etc., são bons tópicos.

___

Q: Você conhece padrões de integrações com filas/tópicos? Se sim, qual foi sua participação em projetos com esse tipo de integração e quais padrões usou?

ER: Se sim, contar alguns padrões usados e quais problemas resolveram.

___

Q: Você sabe a diferença entre Object, File e Block Storages?

ER: Contar a diferença básica entre eles e cenários de uso.

obs.: uma boa referência: [https://www.redhat.com/en/topics/data-storage/file-block-object-storage](https://www.redhat.com/en/topics/data-storage/file-block-object-storage)

___

Q: Quais aspectos/dimensões você pondera antes de incluir um novo componente num desenho de solução? "Componente" aqui pode ser entendido como qualquer building block significativo para a solução como, por exemplo, Kafka, MongoDB, Kubernetes, Redis, Oracle, uma biblioteca, etc. +

___

ER: Os mais diversos possíveis: orçamento, conhecimento do time, facilidade de encontrar profissionais no mercado que conheçam, capacidade de manutenção, maturidade do produto, ofertas de clients para linguagens de programação, "match" no provedor cloud, etc.

___

Q: Qual sua abordagem em relação a segurança?

ER: Questão super ampla também. Mencionar autenticação/autorização, hardening, topologia de redes, MFA, são bons sub tópicos.

___

Q: Como você comunica sobre as soluções que desenha?

ER: Essa questão é direcionada a cargos de maior responsabilidade (arquitetura, staff eng., etc.) e ajuda a entender o modus operandi da pessoa entrevistada.

___

Q: Me conte sobre algum projeto que considera que falhou e o motivo.

ER: Não ter medo de assumir erros – todos nós cometemos. Boa para ter uma noção sobre como a pessoa lida com erros (teoricamente, pelo menos).

___

Q: Quando você acha que a abordagem "least privilege access" não é adequada.

ER: Quando segurança não for importante e velocidade de desenvolvimento for.

___

Q: Geralmente, quais são as maiores dores da operação de sistemas distribuídos?

ER: No geral, eles são complexos. Troubleshooting, por exemplo é mais difícil.

___

Q: Como uma arquitetura assíncrona pode afetar a experiência do usuário?

ER: Abordar Consistência Eventual.

___

Q: O que acha da abordagem multi-cloud?

ER: Questão super aberta. Dependendo do cargo, é possível detectar bastante maturidade em aspectos como custos, disponibilidade, lock-in, abstrações de provedores, etc.

___

E por aí vai...

Essas questões fazem parte de uma lista infinita e, obviamente, incluí apenas algumas das quais me lembrei.

Normalmente, também incluo questões mais focadas na posição para complementar a entrevista.

___

Me conte aí qual outra questão você acha boa para uma entrevista? Não esqueça de incluir a expectativa de resposta se for uma pergunta aberta.
