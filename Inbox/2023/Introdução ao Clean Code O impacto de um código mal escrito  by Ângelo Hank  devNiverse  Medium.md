---
created: 2023-10-24T14:29:32 (UTC -03:00)
tags: []
source: https://medium.com/devniverse/introdu%C3%A7%C3%A3o-ao-clean-code-o-impacto-de-um-c%C3%B3digo-mal-escrito-921849de5299
author: Ângelo Hank
---

# Introdução ao Clean Code: O impacto de um código mal escrito | by Ângelo Hank | devNiverse | Medium

> ## Excerpt
> Nesse post vamos introduzir o conceito de código limpo segundo alguns programadores bem conhecidos e com experiência, além de falar sobre os custos de se ter um código mal escrito. Todas as…

---
[

![Ângelo Hank](https://miro.medium.com/v2/resize:fill:88:88/0*CKo0zLWer0qT3PeU)



](https://medium.com/@angelohank?source=post_page-----921849de5299--------------------------------)[

![devNiverse](https://miro.medium.com/v2/resize:fill:48:48/1*DaAsbyMdVgvruDbfYXTjrA.png)



](https://medium.com/devniverse?source=post_page-----921849de5299--------------------------------)

Nesse post vamos introduzir o conceito de código limpo segundo alguns programadores bem conhecidos e com experiência, além de falar sobre os custos de se ter um código mal escrito. Todas as informações aqui apresentadas foram retiradas do livro “Código Limpo”, de autoria do grande Robert C. Martin, o “Tio Bob”.

Medição da qualidade de um código, segundo Uncle Bob

## Sobre o autor

Robert C. Martin, mais conhecido como “Tio Bob” (ou _Uncle Bob_, no inglês), é uma figura proeminente na indústria de desenvolvimento de software. Nascido em 1952, ele é um renomado engenheiro de software, autor e palestrante.

Com uma vasta experiência no campo da engenharia de software, _Uncle Bob_ trabalhou em diversos projetos, desde sistemas de grande escala até aplicativos de missão crítica. Ele é um defensor ardente da agilidade e dos princípios ágeis de desenvolvimento, e tem contribuído significativamente para a disseminação das boas práticas de programação e arquitetura de software.

Além de “Código Limpo”, Robert também é autor de livros como “Arquitetura Limpa” e “O Codificador Limpo”, muito renomados e indicados para o público de desenvolvedores.

## Motivos para se ler “Código Limpo”

Segundo _Uncle Bob_, existem 2 motivos para se ler e aprender sobre código limpo, e são eles:

**1 — Elevar a qualidade do código que você escreve:** ao compreender os princípios e as práticas apresentadas no livro, os desenvolvedores podem aplicar esse conhecimento no seu próprio trabalho, criando códigos mais legíveis, modulares e sustentáveis.

Nas palavras do próprio autor: “_Você é programador e deseja se tornar um ainda melhor. Ótimo. Precisamos de programadores melhores._”

**2 — Aprimorar habilidade de leitura de código:** ler código é uma atividade comum na vida de um programador, seja para revisar código de colegas, entender o funcionamento de determinadas bibliotecas ou rotinas, analisar projetos legados, etc. Um código bem escrito é mais fácil de se entender, e “Código Limpo” oferece muitas orientações e exemplos para ajudar a entender padrões de código limpo e códigos não tão bem escritos assim.

## O que é um código limpo?

Durante o capítulo 1 do livro, _Uncle Bob_ pergunta para alguns programadores bem conhecidos o que seria um código limpo, e aqui está um resumo do que cada um considera como tal:

**Bjarne Stroustrup, criador da linguagem C++:** Para Bjarne o código deve seguir 4 princípios:

-   Lógica direta para dificultar o encobrimento de bugs;
-   Dependências mínimas para facilitar a manutenção;
-   Tratamento de erros completo de acordo com uma estratégias definida;
-   Desempenho próximo do mais eficiente para não incitar outros programadores a tornarem o código confuso usando otimizações duvidosas e sorrateiras.

Além desses princípios, Bjarne afirma que, para ser considerado um código limpo, deve se ter elegância e eficiência, dando a entender que o código limpo, além de funcionar muito bem, proporciona uma leitura natural e sem dificuldades.

Para encerrar a sua definição, ele faz uma afirmação simples, porém muito presente no restante do livro: “_O código limpo faz bem apenas uma coisa.”_.

**Grady Booch, autor de “_Object Oriented Analysis and Design With Applications_”:** Com uma definição parecida com a de Bjarne, porém mais voltada a legibilidade do código, Grady afirma que ler um código deve ser simples e direto, fazendo com que sua leitura se compare a ler uma prosa bem escrita. Além disso, um bom código, para Grandy, deve conter abstrações claras e linhas de controle objetivas.

Nesse ponto _Uncle Bob_ faz a reflexão sobre livros de ficção e literatura bem escritos, onde, através da leitura era possível se ouvir os sons, imaginar claramente os personagens e as situações descritas durante a trama.

Para Robert, um código pode se comparar a um romance literário, onde deve se expor claramente o problema a ser solucionado e ser desenvolvido até um clímax, onde o leitor poderá entender exatamente a proposta inicial daquele código de maneira óbvia.

**Dave Thomas, fundador da OTI e padrinho da estratégia da Eclipse:** Além de compartilhar da opinião sobre um código legível dos demais, Dave trás mais um critério para que um código seja considerado limpo: **os testes**. Para ele não importa quão elegante, legível e acessível um código esteja. Se ele não possui testes unitários, não pode ser considerado um código limpo.

Dave também define que um código bem escrito precisa ser inteligível, já que, dependendo da linguagem em questão, nem todas as informações necessárias podem ser expressas no código em si.

**Michael Feathers, autor de _Working Effectively with Legacy Code_:** Talvez a melhor definição de todas foi feita por Michael, sendo ela:

> Um código limpo sempre parece que foi escrito por alguém que se importava.

Caramba, que definição perfeita! O próprio _Uncle Bob_ assume que um título substituto para seu próprio livro seria “Como se importar com o código”, já que foi esse o objetivo que o levou à escrita do livro em questão.

Um código limpo é um código que foi cuidado por alguém que prestou atenção aos detalhes e o manteve simples e organizado.

Obrigado Michael por essa definição esplendida.

## **Qual o custo de se ter um código mal escrito?**

Independente de quanta experiência você possua programando, é fácil afirmar que alguma vez você já se deparou com um código confuso que o atrasou.

_Uncle Bob_ compara a experiência de analisar um código confuso com andar em um lamaçal, com arbustos e armadilhas para todo lado, em que esperamos e torcemos para encontrar algo que nos direcione e ajude a entender o que está acontecendo ali, mas dificilmente isso acontece e as coisas ficam cada vez mais confusas.

Mas uma verdade precisa ser dita: do mesmo jeito que você já se deparou com um código mal escrito, alguém já se deparou com um código mal escrito por você.

Alguns fatores podem ter influenciado no motivo de você escrever um código ruim, como prazo para entrega, pensar que seu chefe ficaria bravo se demorasse mais para entregar a demanda que prometeu, cansaço de trabalhar na mesma tarefa por muito tempo e só queria terminar rápido, etc. Todos já passamos por isso, e todos prometemos que arrumaríamos aquele código mais tarde. Spoiler: nunca arrumamos.

Robert descreve uma situação em seu livro que ilustra o que pode acontecer em um cenário em que o código da aplicação é sempre mal escrito e nunca limpo.

Conforme os códigos de um projeto são escrito e a aplicação vai ficando mais complexa, depois de algum tempo os desenvolvedores começam a perceber que a evolução do sistema fica mais lenta, e isso acontece por conta de que uma alteração pontual acaba causando falhas em outras partes do sistema, e é chegado em um ponto em que cada modificação exige que sejam feitas correções e “remendos”.

Em determinado ponto do projeto, por conta da baixa produtividade nas entregas das demandas, a gerência do projeto se vê obrigada a contratar mais mão de obra, a fim de diminuir o tempo para que as alterações sejam entregues. Parece uma boa solução, mas a verdade é que esses novos desenvolvedores não conhecem o sistema, e é muito mais provável que façam alterações que vão impactar negativamente e gerar mais bugs, podendo estes serem descobertos apenas quando a aplicação estiver em produção. Aqui temos outro grande problema.

Como a paciência de todo mundo em algum momento chega ao fim, a equipe informa à gerencia que não consegue mais trabalhar no projeto e exigem um replanejamento e, por conta da (novamente) baixa produtividade, a gerencia concorda.

Uma nova equipe é selecionada para construir a nova versão do sistema, e é responsável por criar um produto que faça as mesmas coisas que o antigo, além de agregar as novas funcionalidade. O detalhe é que a empresa não vai tirar o produto antigo de circulação até que o novo esteja 100% pronto, e isso pode levar **muito** tempo.

Ao final desse processo, você terá a equipe do segundo projeto, que deveria ser melhor, no mesmo caso da primeira, pedindo um replanejamento, e tudo se repete.

Levando tudo isso em consideração, podemos listar os principais custos de se ter um código ruim.

-   Custo de manutenção;
-   Custo de depuração;
-   Baixa produtividade da equipe;
-   Dificuldade de colaboração;
-   Retrabalho constante;
-   Dificuldade de escala;
-   Maior chance de erros em produção;

## Conclusão

Poderíamos aqui dizer que os códigos mal escritos estão presentes na maioria dos sistemas mundo a fora por diversos motivos, como requisitos que mudam e alteram completamente o projeto inicial, gerentes com seus prazos curtos, ou até mesmo colocar a culpa nos clientes intolerantes.

A verdade é que a culpa por um código mal escrito, apesar dos fatores listados acima, é **_nossa_**.

Gerentes buscam nos **desenvolvedores** informações que são necessárias para que eles possam firmar prazos e compromissos, e mesmo quando não buscam, estando dentro do projeto, participando dele, é necessário que se tenha um nível de comprometimento que chega ao ponto de dar uma opinião. Além disso, a responsabilidade por falhas, especialmente se forem causadas por um código ruim, também é de quem desenvolveu a aplicação.

_Uncle Bob_ faz uma comparação no mínimo interessante sobre a relação prazo x qualidade:

Caso você fosse um médico se preparando para uma cirurgia e seu paciente lhe dissesse que não precisa lavar as mãos porque isso levaria muito tempo, o que você faria?

É obvio que o médico deve se recusar a obedecer, afinal, ele entende mais de doenças infecciosas que o seu paciente.

Substitua o paciente pelo chefe pedindo um prazo mais curto e o médico por um desenvolvedor que entende dos riscos de se ter um código mal escrito.

Gerentes podem ter paixão e proteger o produto prometendo prazos e requisitos, essa é a função deles. A função de proteger o código é do programador, e deve ser feita com a mesma paixão.

## Bônus: A Regra do Escoteiro

Por mais bem escrito que seja um código, com certeza ele irá sofrer alterações, seja por necessidade de manutenção ou novas _features_ que serão desenvolvidas. Códigos estragam e se degradam com o tempo, isso é inevitável.

Com isso em mente, fica claro que não basta apenas escrever bons códigos, é preciso ter um papel ativo na prevenção da degradação, não deixando que se chegue no ponto de se tornar um código ruim.

A maior organização de escoteiros dos EUA, _Boy Scouts of America_ tem uma regra muito simples e que pode ser aplicada no contexto que estamos falando:

> **Deixe a área do acampamento mais limpa do que você a encontrou**.

Se cada um dos programadores deixasse o código em que está trabalhando um pouco mais limpo do que encontrou, a degradação não aconteceria, ou pelo menos evitaria que a “limpeza” fosse tão grande. Defina nomes melhores para variáveis, separe funções que estão grandes demais, elimine um pouco de repetição de código. Tarefas simples e que podem evitar que o código do projeto em que está trabalhando fique ultrapassado e “mal escrito”.
