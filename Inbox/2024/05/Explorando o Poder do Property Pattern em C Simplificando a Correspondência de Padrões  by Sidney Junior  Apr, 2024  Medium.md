---
created: 2024-05-01T09:37:42 (UTC -03:00)
tags: []
source: https://medium.com/@sidneyjunior26/explorando-o-poder-do-property-pattern-em-c-simplificando-a-correspond%C3%AAncia-de-padr%C3%B5es-4eb406d55f0f
author: Sidney Junior
---

# Explorando o Poder do Property Pattern em C#: Simplificando a Correspondência de Padrões | by Sidney Junior | Apr, 2024 | Medium

> ## Excerpt
> A correspondência de padrões é uma técnica poderosa em programação que permite escrever código mais expressivo e conciso. No C# 9.0 e posterior, uma adição notável ao arsenal de recursos dessa…

---
[

![Sidney Junior](https://miro.medium.com/v2/resize:fill:88:88/1*YBvcOCIaRzMdf-49_keXPw.jpeg)



](https://medium.com/@sidneyjunior26?source=post_page-----4eb406d55f0f--------------------------------)

![](https://miro.medium.com/v2/resize:fit:875/1*Rlqxy_QJBrF_uvfY8fWDzw.png)

A correspondência de padrões é uma técnica poderosa em programação que permite escrever código mais expressivo e conciso. No C# 9.0 e posterior, uma adição notável ao arsenal de recursos dessa linguagem é o “Property Pattern” (padrão de propriedade). Este recurso oferece uma maneira elegante de fazer a correspondência de padrões com base nas propriedades dos objetos. Neste artigo, exploraremos o que é o property pattern, como funciona e como pode ser usado para tornar seu código mais intuitivo e dinâmico.

## Afinal, o que é o Property Pattern?

O property pattern é uma técnica de correspondência de padrões que permite verificar as propriedades de um objeto em tempo de execução. Ele é especialmente útil quando você precisa fazer a correspondência de padrões em estruturas de dados complexas sem ter que descompactar manualmente os valores de suas propriedades.

## Como Funciona?

Em sua essência, o property pattern permite que você especifique padrões diretamente nas propriedades de um objeto. Isso é feito usando a sintaxe `{ Propriedade: valor }`, onde `Propriedade` é o nome da propriedade que você deseja verificar e `valor` é o valor que você espera que a propriedade tenha.

## Exemplos de Uso — Verificando o Tipo de Animal

Imagine que estamos desenvolvendo um programa que lida com diferentes tipos de animais. Cada animal tem um nome e uma categoria, como “mamífero”, “réptil” ou “ave”. Vamos usar o property pattern para determinar o som que cada animal faz com base em sua categoria.

![](https://miro.medium.com/v2/resize:fit:875/1*J6R0h-J-Jsslzck55JWg5A.png)

Neste exemplo, estamos usando o property pattern para fazer a correspondência da categoria de animal com diferentes padrões e determinar o som com base nisso.

## Utilizando instrução “if”:

![](https://miro.medium.com/v2/resize:fit:875/1*c5dYT4v_v1Sn3-2n53Z8bg.png)

Neste exemplo, adicionamos a propriedade `Age` ao objeto `Animal` para representar a idade do animal. Em seguida, usamos o property pattern para verificar tanto a categoria quanto a idade do animal para determinar o som que ele faz. Se o animal for um mamífero e tiver menos de 2 anos, consideramos um filhote; se for um mamífero e tiver 2 anos ou mais, consideramos um adulto.

## Benefícios do Property Pattern:

-   Concisão: Permite escrever código mais conciso, eliminando a necessidade de descompactar manualmente os valores das propriedades.
-   Legibilidade: Torna o código mais legível e intuitivo, especialmente ao lidar com estruturas de dados complexas.
-   Dinamismo: Facilita a criação de lógica dinâmica com base nas propriedades dos objetos.

## Conclusão:

O property pattern é uma adição valiosa ao C# que simplifica a correspondência de padrões em estruturas de dados complexas. Ao usar o property pattern, você pode escrever código mais expressivo, conciso e dinâmico, tornando seu processo de desenvolvimento mais eficiente e agradável.

Experimente este recurso em seus próprios projetos e descubra como ele pode melhorar a qualidade e a legibilidade do seu código.
