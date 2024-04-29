---
created: 2023-11-03T16:45:39 (UTC -03:00)
tags: [beginners,programming,career,ocaml,software,coding,development,engineering,inclusive,community]
source: https://dev.to/renatoalencar/por-que-aprender-ocaml-38dc
author: Renato Alencar
---

# Por que aprender OCaml? - DEV Community

> ## Excerpt
> Quando eu decidi aprender OCaml, uma das primeiras palestras que assisti foi a Por que OCaml? do...

---
Quando eu decidi aprender OCaml, uma das primeiras palestras que assisti foi a [**Por que OCaml?** do Yaron Minsky](https://youtu.be/v1CmGbOGb2I?si=aI4ji7qH59V8E8a9). Onde ele apresenta algumas das razões pelas quais a Jane Street escolheu OCaml como a linguagem que iriam usar para construir quase tudo dentro da empresa. O que é interessante, é que OCaml foi uma linguagem que escolhi por motivos bem específicos e pensados que se mantiveram verdade com o tempo e experiência com a linguagem.

## [](https://dev.to/renatoalencar/por-que-aprender-ocaml-38dc#programa%C3%A7%C3%A3o-funcional)Programação funcional

Há alguns anos que tenho focado bastante em programação funcional, que acredito ser uma melhor forma de escrever software que suas contrapartes. Apesar de se poder usar elementos de programação funcional em linguagens como Python e Ruby, os idiomas dessas linguagens ainda são voltados para um modelo mais baseado em Orientação a Objetos, o que torna os idiomas funcionais muitas vezes quase impossíveis de entender.

## [](https://dev.to/renatoalencar/por-que-aprender-ocaml-38dc#python-e-clojure)Python e Clojure

Python foi a primeira linguagem que dominei com maior profundidade, lá em 2012, desde então tenho um certo amor pela simplicidade. Muita dessa simplicidade vem das influencias de Lisp que a linguagem possui.

Clojure já sendo funcional e com uma sintaxe próxima do Lisp, me chamou atenção facilmente, passei bastante tempo focado aprendendo os idiomas da linguagem. Mas essencialmente faltavam algumas coisas, como um bom sistema de tipos (tipos e Clojure é uma toca do coelho bem funda) e a dependencia na JVM (o que é bom no tipo de domínio onde Clojure é presente como linguagem). Além disso, o tooling é razoavelmente complicado, falta documentação em algumas coisas e a comunidade tende a ser muito focada em enterprise ou consultoria.

## [](https://dev.to/renatoalencar/por-que-aprender-ocaml-38dc#haskell)Haskell

Tentei gostar de Haskell, mas apesar de Haskell ser interessante, algumas coisas pesam bastante. Todo o tooling tende a ser pesado, exemplificando a imagem Docker tem 700 MB (na época passava de 1GB). Além disso, há um certo overhead até conseguir fazer algo mais prático, algo que já é mais fácil em Clojure por exemplo com estado, usando apenas um `atom`.

## [](https://dev.to/renatoalencar/por-que-aprender-ocaml-38dc#entra-ocaml)Entra OCaml

OCaml já faz as coisas de uma forma um pouco diferente, a sintaxe mais próxima do ML com a possibilidade de rodar side-effects sem muita complicação ajuda a quem está tentando aprender, além de facilitar experimentação e um design mais progressivo.

Não é necessário entender a monad `IO` do Haskell para poder fazer um simples Hello World, mas é possível rodar tudo atrás de monads se for do interesse de quem tá programando.  

```
main :: IO ()
main = putStrLn "Hello World"
```

Enter fullscreen mode Exit fullscreen mode

```
let () = print_endline "Hello World"
```

Enter fullscreen mode Exit fullscreen mode

`print_endline` é uma chamada direta, não há abstrações extras bem pouco overhead para entender essa única linha de código.

Se eu possuir algum tooling de OCaml instalado localmente, eu ainda consigo compilar um arquivo `.ml` com um simples `ocamlc` sem precisar criar um projeto inteiro do zero só para fazer pequenos experimentos como em Clojure. O Babashka resolve esse problema, mas adiciona mais um overhead para quem está aprendendo.

## [](https://dev.to/renatoalencar/por-que-aprender-ocaml-38dc#tipos-bons)Tipos (bons)

Muita gente não gosta de tipos, Rich Hickey que o diga. E eu particularmente acho que TypeScript cria várias dificuldades se você tá só tentando implementar algo simples, além de ter um sistema de tipos que eu particularmente acho desagradável, subtipagem estrutural cria mensagens de erro péssimas com objetos grandes.

Além disso, entra outro problema: ter que sempre declarar os tipos se você quiser que o compilador verifique o você quer. Isso graças a necessidade de se juntar tipos abstratos com subtipagem.

Em Python por exemplo, a seguinte função vai ter que usar `Any` para `n` e para o valor de retorno:  

```
def fib(n):
    if n < 2: return n
    return fib(n - 1) + fib(n - 2)
# def fib(n: Any) -> Any
```

Enter fullscreen mode Exit fullscreen mode

Já o seguinte código em OCaml tem seus tipos inferidos sem problema algum:  

```
let rec fib n =
    if n < 2 then n
    else fib (n - 1) + fib (n - 2)
(* val fib : int -> int = <fun> *)
```

Enter fullscreen mode Exit fullscreen mode

## [](https://dev.to/renatoalencar/por-que-aprender-ocaml-38dc#erros-ficam-%C3%B3bvios)Erros ficam óbvios

Linguagens funcionais em geral tem essa propriedade de deixar aquele código que provavelmente não deveria ser feito que você fez mais óbvio. Com um bom sistema de tipos, isso fica ainda mais óbvio.

Por exemplo, serializers deveriam ser funções puras, e não fazer consultas ao banco. Por questões óbvias de performance, de manutenção, e raramente alguem espera que um serializer esteja fazendo um consulta ao banco.

Em OCaml, para sistemas em produção, normalmente utilizamos [Lwt](https://github.com/ocsigen/lwt), que é basicamente um sistema de promises para IO assíncrono e concorrência. Acontece o tipo `Lwt.t` é uma mônada, e mônadas tem essa propriedade interessante de "contaminar" tudo que elas tocam. Vamos supor que seu serializer tenha a seguinte assinatura, você converte um dado do tipo `t` para uma string a ser enviada para outro serviço (provavelmente JSON).  

```
type t

val serialize : t -> string
```

Enter fullscreen mode Exit fullscreen mode

Se você só precisa converter a informação pra `string`, é essa a assinatura que o seu serializer deve ter. Porém, se você precisa fazer uma consulta ao banco dentro de `serialize`, por qualquer que seja o motivo, a função é forçada a ter outra assinatura:  

```
val serialize : t -> string Lwt.t
```

Enter fullscreen mode Exit fullscreen mode

O que torna óbvio que `serialize` tá fazendo algo que não deveria.

## [](https://dev.to/renatoalencar/por-que-aprender-ocaml-38dc#performance)Performance

Outro fator importante, é performance. OCaml tem performance bem previsível, se você chegar a explorar um pouco sobre como o próprio compilador funciona, e como seu código final fica em Assembly na arquitetura em uso. Você acaba percebendo com o tempo, que você é capaz de perceber de antemão com uma certa facilidade como que o seu código vai ser executado em máquina.

O compilador do OCaml já é bem conhecido por emitir código bem eficiente, o que é muito bom pra uma linguagem com o nível de abstração que o OCaml oferece.

Talvez um exemplo clássico seja Fibonacci, veja como emite código eficiente:  

```
let rec fib n a b =
  if n < 2 then b
  else fib (n - 1) b (a + b)
```

Enter fullscreen mode Exit fullscreen mode

OCaml usa o bit menos significativo pra diferenciar inteiros de ponteiros e fazer cálculo com inteiros unboxed. O código fica um pouco diferente do que se esperaria, mas ainda assim muito performático:  

```
camlExample__fib_268:
        subq    $8, %rsp
.L101:
        cmpq    (%r14), %r15
        jbe     .L102
.L103:
        cmpq    $5, %rax
        jge     .L100
        movq    %rdi, %rax
        addq    $8, %rsp
        ret
.L100:
        leaq    -1(%rbx,%rdi), %rsi
        addq    $-2, %rax
        movq    %rdi, %rbx
        movq    %rsi, %rdi
        jmp     .L101
.L102:
        call    caml_call_gc@PLT
.L104:
        jmp     .L103
```

Enter fullscreen mode Exit fullscreen mode

Observe alguns detalhes:

-   Todo o cálculo é feito diretamente nos registradores;
-   A recursão é convertida em um loop de forma eficiente;
-   Os registradores usados são os mesmos dos parâmetros da convenção de chamada.

## [](https://dev.to/renatoalencar/por-que-aprender-ocaml-38dc#ecossistema)Ecossistema

O ecossistema é bem acessível se você souber inglês pelo menos, o [forúm](https://discuss.ocaml.org/) é bem movimentado e as pessoas envolvidas tanto na linguagem quanto nas bibliotecas quase sempre respondem as pessoas lá mesmo. O que é bem legal, já que você tem a perspectiva diretamente das pessoa que trabalham naquilo ao invés de outro usuário direto da linguagem ou biblioteca.

## [](https://dev.to/renatoalencar/por-que-aprender-ocaml-38dc#por-onde-come%C3%A7ar)Por onde começar?

-   ocaml4noobs é muito bom pra quem quer começar em Português: [https://github.com/Camilotk/ocaml4noobs](https://github.com/Camilotk/ocaml4noobs)
-   Real World OCaml já te dá um conteúdo bem mais avançado com casos de usos mais práticos: [https://realworldocaml.org/](https://realworldocaml.org/)
-   OCaml Programming: Correct + Efficient + Beautiful com conhecimentos teóricos de Ciência da Computação aplicados à linguagem: [https://cs3110.github.io/textbook/cover.html](https://cs3110.github.io/textbook/cover.html)
