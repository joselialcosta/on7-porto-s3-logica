# Aula 3 - Funções, escopo e loops

Nesta aula vamos tratar de mais conceitos *importantes* de lógica de programação com JavaScript. Procure ler com calma e testar livremente os exemplos para internalizar bem!

[Funções](##Funções)
* [`return` x `console.log()`](###Return)
* [Declaração de função x expressão de função](###Formas)
* [Arrow Function](###Arrow)
* [IIFE](###iife)
* [Dicas para escrever funções](###Dicas)
* [Links e referências](###Links)

[Escopos](##Escopos)
* [Escopos aninhados](###Nested)
* [Sombreamento (*shadowing*)](###Sombreamento)
* [Declaração de `let` em escopos locais](###let)

[Loops](##Loops)
* [while](###while)
* [do... while](###do)
* [for](###for)
* [break](###break)
* [Diferenças entre `for` e `while`](###diferenças)

## Funções

O que é uma função?
Uma função é um bloco de código/instruções. Usando funções, podemos "chamar" o código que queremos executar, quando queremos executar, e dando para o código as informações que ele precisa.
Por exemplo:

```js
//uma função que receba dois números e faça uma soma

//primeiro momento: declarar a função - ou seja, escrever o que ela faz
function soma(numero1, numero2) {
  return numero1 + numero2
}

//segundo momento: "chamar" a função quando queremos que ela seja executada
soma(1, 1)
//resultado: 2
```

Essa é a estrutura de uma função "clássica". Existem outros jeitos de declarar funções mas vamos entender esse primeiro.
- `function nomeDaFuncao(parametro1, parametro2)`: A palavra-chave `function` avisa o JS que vamos começar a escrever uma função aqui. Em seguida, `nomeDaFuncao` é o nome que vamos dar para ela. Você pode dar o nome que quiser, só não esqueça de seguir a convencaoDeNomesCamelCase 🐪 e de dar pra função **um nome que faça sentido** - ou seja, que diga o que a função faz.
- O trecho entre parênteses são os **parâmetros** `(parametro1, parametro2)`. Você pode dar o nome que quiser também, desde que façam sentido. Os parâmetros são *muito importantes* pois é através deles que a função recebe as informações que precisa para fazer o que queremos. **Importante também**: as funções podem não receber parâmetro nenhum, caso não precise, ou receber quantos precisar. Não tem número mínimo nem máximo.
- A palavra-chave `return` também é muito importante: é ela que "manda pra fora" da função a informação que queremos. Sem o retorno, a função pode fazer várias tarefas, mas nenhum dado que ela processar vai poder ser acessado pelo restante do código.
- Os "momentos": Lembra que usamos funções para que certos trechos de código só sejam executados no momento certo? Então a função também tem dois momentos. No primeiro, quando ela é declarada, escrevemos tudo: nome, o que faz, o que retorna. No segundo momento, quando "chamamos" a função passando os valores que ela precisa, é que ela é executada. *A função não vai ser nunca executada se não chamarmos!* 

Abaixo temos mais exemplos para entendermos melhor cada caso.

1. Função sem retorno e sem parâmetro:
```js
function olar(){
  console.log('oi gente!')
}

olar()
```

2. Função sem retorno, com parâmetro:
```js
function olarPessoa(pessoa){
  console.log(`oi, ${pessoa}!`)
}

olarPessoa('Helena')
```

3. Função com retorno, sem parâmetro:
```js
function escreverOlar(){
  return 'oi gente!'
}

function escreverOlarPraAlguem(nomePessoa) {
  console.log(`${escreverOlar()} Meu nome é ${nomePessoa}`)
}

escreverOlarParaAlguem('Helena')
```

4. Função com mais de um parâmetro:
```js
function operacaoMatematica(numero1, numero2, numero3) {
  return numero1 + numero2 + numero3
}

operacaoMatematica(1, 1, 1)
```

### Return

Qual a diferença entre `return` e `console.log()`?

O `console.log` significa, traduzindo do inglês, "registro no console". Ou seja, é somente um registro pra gente que está desenvolvendo obter alguma informação do código, mas *o `console.log()` não influencia no código, é só pra dar informação!*

Já o `return` é o comando que usamos quando realmente precisamos que a função "mande pra fora dela" algum dado que precisamos usar em outra parte do código. Ele deve ser sempre a última coisa a ser escrita na última linha antes de fechar a função, pois tudo que vem depois desse comando é ignorado pelo JS.

Em alguns dos casos acima (casos 1 e 3), a própria função já define `console.log()` então a informação já vai ser exibida no console. Já no caso 4, se quisermos conferir o retorno da função no console, devemos chamá-la no formato: `console.log(operacaoMatematica(1, 1, 1))`.

### Formas
**Declaração x Expressão**

A forma que acabamos de ver (que chamei de "clássica") é a que chamamos de declaração de função, com a palavra-chave `function` e depois o nome que damos pra função.

Outra forma de escrever funções é dessa forma:

```js
const olar = function() {
  console.log('oi gente!')
}

olar()
```

Ou, utilizando parâmetros:

```js
const soma = function(numero1, numero2) {
  return numero1 + numero2
}

console.log(soma(1, 1)) //resultado: 2
```

Qual a diferença? 

Nessa versão, a função em si não tem nome (é uma função anônima) e chamamos através de uma variável. De resto, escrevemos de forma parecida.

A diferença está num comportamento do JS chamado içamento, ou *hoisting*. Quando o arquivo JS é carregado, todas as funções declaradas (que têm nome) são içadas, ou puxadas para o topo co contexto.

O que? Traduzindo, é como se, quando o arquivo fosse carregado, o JS puxasse para o começo do código todas as funções que têm nome e já lisse todas elas. Então não importa em que parte do código elas são chamadas, o JS já sabe o que elas fazem.

Já as expressões (esse último caso que vimos) são anônimas, então o JS não sabe o que elas fazem até que chegue na linha certa. Na prática:

```js

funcaoDeclarada()

function funcaoDeclarada() {
  console.log('essa função já foi carregada!')
}

```

Você pode tranquilamente chamar a função antes de declarar o que ela faz, pois o JS quando carregar o arquivo vai *primeiro* puxar pro topo as funções nomeadas, ler o que elas fazem e aí então executar o código.

Já no caso das expressões de função:

```js
expressaoDeFuncao()

const expressaoDeFuncao = function() {
  console.log('será que funcionou?')
}

```
Se você rodar o código como está, vai receber um erro do tipo `expressaoDeFuncao is not defined` ("expressaoDeFuncao não está definido") porque o JS não consegue chamar uma função antes de ler o que ela faz, e uma coisa está depois da outra. Troque as linhas de lugar e tudo volta a funcionar!

### Arrow
**Arrow Functions (função seta)**

Por fim, uma última forma (por enquanto!) de se escrever funções. Essa forma veio com as implementações mais recentes do JS, o tal ES6 ou JS2015 e é chamada de *arrow function*, por ser caracterizada pela `=>` (arrow, ou seta/flecha).

Essa forma pode a princípio parecer um pouco estranha de escrever, mas você vai se acostumando! Comece pela forma que acha mais confortável.

A *arrow function* é uma outra forma de se escrever somente **expressões de função**.

O jeito tradicional de escrever:
```js
const soma = function (num1, num2) { 
  return num1 + num2
}
console.log(soma(1, 1)) //2
```

Utilizando *arrow function*:
```js
const soma = (num1, num2) => num1 + num2
console.log(soma(1, 1)) //2
```

Nessa versão:
- não utilizamos a palavra-chave `function`
- a seta `=>` que indica função vai *depois* dos (parâmetros)
- como o código da função só tem uma linha, não precisamos abrir e fechar chaves `{}` e nem escrever `return` (fica implícito)

Caso a função tenha mais de uma linha, aí sim precisamos abrir chaves e escrever `return`, mesmo usando a `=>`:
```js
const imprimirSomaEPessoa = (num1, num2, nome) => {
  const resultadoSoma = num1 + num2
  return `resultado: ${resultadoSoma}, pessoa: ${nome}`
}
console.log(imprimirSomaEPessoa(1, 1, 'Helena'))
```

### iife
**IIFE (Immediately Invoked Function Expression), ou funções imediatas**

É uma forma de declarar uma função e já executá-la em seguida, sem precisar chamar a função em uma outra linha de código. Pode funcionar tanto com expressões ou com funções declaradas. Basta envolver toda a função com parênteses e incluir parênteses vazios no final:

```js
const imprime = (function () { 
    let nome = 'Helena' 
    return nome; 
})()
console.log(imprime)
```
Nesse exemplo, a variável `imprime` não guarda a função e sim *somente seu resultado*. O que está sendo exibido no console em `console.log(imprime)` é justamente esse resultado.

Vamos ver outro exemplo:
```js
(function imprime() { 
    let nome = 'Helena' 
    console.log(nome)
})()
```

Também vai funcionar com funções nomeadas, mas o normal é utilizar com *expressões*.
Você também pode escrever com os parênteses vazios para dentro, dessa forma.
```js
const imprime = (function () { 
    let nome = 'Helena' 
    return nome; 
}())
```

### Dicas
Algumas dicas de funções que você pode levar pra vida:

Listamos alguns princípios que te vão te ajudar a escrever funções melhores:

* **Don't Repeat Yourself (DRY)**: É muito comum que a gente identifique um padrão que se repete ao longo de nosso código.

Uma vez identificado um padrão - por exemplo, se você escreveu dois pedaços de código muito parecidos, é hora de escrever uma função que faça um "modelo" do padrão encontrado para que seja reutilizado facilmente.

* **Do One Thing (DOT)**: Cada função deve fazer somente uma coisa e fazê-la o melhor possível. Seguindo este princípio, você escreverá funções mais reutilizáveis, legíveis e fáceis de debugar.

* **Keep It Simple Stupid (KISS)**: Se as funções devem fazer somente uma coisa, quanto mais claro e mais simples, melhor. Nem sempre é fácil fazer isso, especialmente quando estamos começando. Praticar sempre! 

* **Less Is More**: Para serem o mais legíveis possível e reduzir a
  tentação de fazer mais de uma coisa, as funções devem ser tão pequenas quanto for possível. Se a função fica muito longa, é melhor considerar separá-la.

### Links

Alguns links e referências.
Esses links têm bastante conteúdo, vá se familiarizando aos pouquinhos!

- [Funções](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Fun%C3%A7%C3%B5es) no MDN
- [Vários exemplos](https://braziljs.org/artigos/funcoes-em-javascript/) de funções no site da BrazilJS. Algumas informações são específicas do JS no front-end, então não se preocupe se tiver muita coisa desconhecida!
- [Capítulo inteiro de funções](https://braziljs.github.io/eloquente-javascript/chapters/funcoes/) do livro JavaScript Eloquente


***

## Escopos

Escopo é como chamamos o contexto onde as coisas acontecem no código. Ou seja, onde cada trecho de código é lido, interpretado e pode ser acessado.

Antes, no JavaScript podíamos apenas criar um novo escopo quando criavamos
uma nova função. Mas desde a atulização do ES6 (ES2015) temos a `let` e a
`const`, que introduziram o conceito de *escopo de bloco* no JavaScript. Um bloco de código é definido por estar entre chaves `{}`.

O tema de escopo é complexo, então vamos ver um exemplo prático:

```js
const exemplo = () => {
  let x = 1
}
exemplo()
console.log(x)
```

Aqui, o escopo direto de `x` é a função `exemplo`. A variável `x` poderá ser acessada apenas dentro do bloco da função `exemplo`, mas não fora dela. Faça o teste com esse código, vai ver o erro `x is not defined`. Isso acontece porque `let x` foi criada *dentro do escopo de bloco da função `exemplo`* e só consegue ser acessada (ou visível) dentro da função - ou seja, entre as chaves `{}`.

Vamos ver de novo:

```js
const exemplo = () => {
  let x = 1
  console.log(x)
}
exemplo()
```
Agora sim.

### Nested 
**Nested Scopes (escopos aninhados)**

Se o escopo está aninhado dentro do escopo direto de uma variável, a
variável será acessível a todos os escopos:

```js
function textoExterno(texto) {
  console.log(texto)
  function textoInterno() {
    const frase = 'texto da função aninhada'
    console.log(frase)
  }
  textoInterno()
}
textoExterno('olar')
```

### Sombreamento

É possível declarar uma variável com o mesmo nome em um escopo interno
de uma função, com isso o acesso à variável externa é bloqueado no escopo
interno e todo os escopos aninhados dentro dela. Mudanças nas variáveis internas
não afetam a variável externa, que é acessível fora do escopo interno.
Exemplo:

```js
let variavel = 'global';
function exemplo() {
  let variavel = 'local';
  console.log(variavel); // local
}
exemplo();
console.log(variavel); // global
```

Dentro da função `exemplo`, a variável global `variavel` é "sombreada" pela variável local `variavel`.

### let 
**Declaração de variáveis no escopo local com `let`**

A "palavra" `let` declara uma variável de alcance local. Ela pode, opcionalmente,
ser iniciada com algum valor e pode ser reatribuída (diferente de `const`).

O alcance da `let` é local ao bloco, a declaração ou expressão onde se está
usando. O que diferencia a palavra `let` da palavra `var` é que `var` não respeita o escopo de bloco e pode "escapar" quando menos esperamos (causando bugs).

Alguns exemplos:

```js
(function () {
  if (true) {
    let frase = 'Olar mundo';
  }
  console.log(frase);
})()
```
O exemplo acima vai dar erro, porque `let` só existe dentro do *escopo de bloco* do `if` (lembrando que um bloco de código é definido pelas `{}` e não precisa ser necessariamente uma função!).

Se tentarmos novamente com `var`:
```js
(function () {
  if (true) {
    var frase = 'Olar mundo';
  }
  console.log(frase);
})()
```
Agora a função roda normalmente e exibe a mensagem, pois `var`, ao contrário de `let` e `const` não liga muito pra escopo.

Bom, então `var` é melhor né? **Não!** Nós normalmente não queremos esse comportamento, pois quanto mais "cercadas" as variáveis estiverem no código, menor a chance de comportamentos inesperados e de encontrarmos valores que não esperamos dentro das variáveis.

Então, nada de `var`! As variáveis `let` e `const` surgiram justamente pra atender a essa necessidade de maior organização no código.

### Links

- Uma geral sobre [escopos](https://pt.wikipedia.org/wiki/Escopo_(computa%C3%A7%C3%A3o)) na Wikipedia

***

## Loops
**Loops ou laços de repetição**

Os *loops* são estruturas repetitivas, que permitem executar um código várias vezes,
dependendo se uma condição **continua sendo** verdadeira.

Imagine um programa que imprima todos os números pares do 1 ao 12. Uma maneira
de escrevê-lo seria assim:

```js
console.log(0)
console.log(2)
console.log(4)
console.log(6)
console.log(8)
console.log(10)
console.log(12)
//   … etcetera
```

Isso funciona, mas a ideia de escrever um programa é trabalhar menos, e não
mais. E se fosse uma lista com 1000 números? Ficaria impossível.

O que precisamos aqui é dar um jeito de repetir partes de código. Esta forma de **controle de fluxo** é chamada de loop. Os loops permitem voltar a certo ponto no programa em que estivemos antes e repetir qualquer operação a partir do estado em que estamos.

### while
**Loop `while`**

O loop mais simples é o loop `while` (que significa "enquanto" em português). Um
loop `while` executa repetidamente uma série de instruções até que uma condição
particular deixe de ser verdadeira. Ao escrever um loop `while`, você está
dizendo: "Continue fazendo isto enquanto esta condição seja verdadeira. Pare
quando a condição se tornar falsa."

```js
while (condição) {
  // Conjunto de sentenças, onde
  // se inclui algo que "muda" para
  // que a condição eventualmente seja FALSA
}
```

O loop executa o comando enquanto a condição produza um valor que seja `true`. Por isso é muito importante que o conjunto de comandos inclua algo que *"modifique"* para que a condição eventualmente seja falsa. Do contrário, vai gerar o que chamamos de "loop infinito", e vai dar ruim (travar seu terminal ou seu computador).

Vamos voltar agora ao problema de imprimir todos os números pares do 1 ao 12, agora com `while`:

```js
let numero = 0;
while (numero <= 12) {
  console.log(numero);
  numero = numero + 2;
}
```
Vamos ver o que acontece aqui:
- Criamos uma variável `numero`, inicializamos com o valor 0, e a utilizamos na condição. Lendo a condição, podemos traduzir para "enquanto (while) o valor da variável `numero` for menor ou igual a 12.
- O bloco do `while` inclui duas sentenças: a primeira imprime o número (com `console.log`) e a segunda incrementa `numero` em 2 (porque
queremos imprimir só os pares). 
- A variável `numero` demonstra a forma em que uma variável pode dar seguimento ao progresso de um programa. Cada vez que o loop se repete, `numero` se incrementa em 2. Então, **no início de cada repetição**, o valor da variável `numero` é comparado com o número 12 para decidir se o programa fez todo o trabalho que deveria fazer. 

É importante entender que, se não modificamos o valor de `numero` com a segunda sentença, a condição `(numero <=12)` sempre será `true` e teremos um loop infinito.


### do
**Loop `do`**

O loop `do` é uma estrutura similar ao loop `while`. A diferença está em um só ponto: um loop `do` sempre executa o que está dentro do bloco *pelo menos uma vez* e começa a verificar se devería parar somente depois da primeira execução.
Assim, a condição aparece depois do bloco do loop:

``` js
let numero = 2
do {
 console.log(numero)
} while (numero < 2)
```
A palavra `do` significa "fazer" em inglês, então podemos ler o bloco acima dessa forma: "*faça* {o que está no bloco} *enquanto* a variável `numero` for menor que 2". Teoricamente esse bloco não deveria fazer nada, pois a variável `numero` já começa declarada com valor 2, mas como o loop `do` sempre roda o que está dentro do bloco pelo menos uma vez, vai imprimir `2` no console pelo menos uma vez antes de ver o que tem no "enquanto" (`while`). 

### For

O `for` é um loop mais completo e é bastante utilizado para situações mais complexas, quando precisamos fazer um controle mais "refinado" do que acontece em cada loop (ou iteração).

Vamos ver a estrutura do `for` "clássico" com o mesmo caso de imprimir números pares de 1 a 12: 

```js
for (let numero = 0; numero <= 12; numero = numero + 2){
  console.log(numero);
}
//0
//2
//etc
```

Hora de ver com calma o que acontece em cada parte desse código:
- O primeiro trecho dentro do parênteses (`let numero = 0`) inicia o loop, normalmente definindo uma variável que vai ser alterada durante a execução do código.
- O segundo trecho (`numero <= 12`) é a expressão que verifica se o loop tem que continuar. Cada vez que o JS chega ao fim do bloco `for` ele volta nesse trecho pra verificar se a condição ainda é `true`.
- O último trecho atualiza o estado do loop antes de cada nova iteração.
- Os três trechos dentro do parêntese devem sempre ser separados por ponto-e-vírgula (`;`). 

Agora vamos ver um código que calcula 2^10 (2 exponencial 10), usando `for`:

```js
let resultado = 1
for (let contador = 0; contador < 10; contador++){
  resultado = resultado * 2
}
console.log(resultado)
// → 1024
```

A representação geral do loop `for` é a seguinte:

```js
for (inicial; condição; incremento){
  Bloco de código a executar
}
```

O *inicial* (por exemplo: `let contador = 0`) se executa antes de que se
inicie o loop. Geralmente se usa para criar uma variável que rastreia o número
de vezes que foi executado o loop.

A *condição* (`contador < 10`) é verificada antes de cada execução do bloco do loop. Se a condição é verdadeira, o bloco é executado; se é falsa, o loop se detém. Neste caso, o loop se deterá uma vez que counter já não seja inferior a 10. 

O *incremento* (`contador++`) é executado depois de cada execução do bloco do loop. Geralmente se utiliza para atualizar a variável do loop. No nosso exemplo, utilizamos para incrementar em 1 (usando o operador `++`) ao contador cada vez que se executa o loop. 

Lembrando que, se não damos ao loop uma condição de parada (por exemplo, incrementando a variável `contador` até que chegue ao valor de `10`) vamos cair no loop infinito!

### Break

Fazer com que a condição do loop produza _false_ não é a única forma de finalizar um loop. Podemos usar a sentença especial `break`, utilizada no `switch`, que tem o efeito de sair imediatamente do loop.

Vamos usar o `break` para encontrar o primeiro número que é maior ou igual a
20 e divisível por 7.

```js
for (let numAtual = 20; ; numAtual++) {
  if (numAtual % 7 === 0) {
    console.log(numAtual);
    break
  }
}
// → 21
```

Usar o operador de resto ou módulo (%) é uma forma fácil de provar se o número é divisível por outro. Se for, o resto da divisão é zero.

O `for` neste exemplo não tem a parte que verifica se o loop deve terminar. Isso significa que o loop não vai parar até que a sentença `break` que está dentro seja executada.

Como já estamos aprendendo, se você deixasse fora essa sentença `break` ou
acidentalmente escrevesse uma condição que sempre produza `true`, o seu programa ficaria travado em um loop infinito.

A palavra chave `continue` é parecida com o `break` pois influencia o progresso do loop. Quando `continue` é lido no bloco de um loop, o controle sai do bloco do loop imediatamente e continua na próxima iteração do loop.

### Diferenças 
**As diferenças entre `for` e `while`**

Resumidamente, usamos o `for` quando sabemos quantas repetições vão ser
realizadas e o `while` quando não sabemos.

Por exemplo, se queremos dar o comando específico "gire as pás do ventilador 10 vezes", você já sabe que vamos girar o ventilador 10 vezes, então pode usar o `for`.

Mas, se queremos dar uma instrução do tipo "enquanto estiver calor gire as pás do ventilador", não sabemos quantas vezes vamos girar o ventilador até a temperatura baixar, então nesse caso é melhor usar o `while`.

### Links

- [Laços e iterações](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Lacos_e_iteracoes) no MDN
- [For](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/for) no MDN