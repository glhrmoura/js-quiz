# Respostas

### Questão 1:

```javascript
('b'+'a'+ +'a'+'a').toLocaleLowerCase()
```

**c) `'banana'`**

Quando temos essa expressão `'b'+'a'+ +'a'+'a'` parece que vai ser retornado `'baaa'` porque só estamos concatenando strings, mas tem um detalhe que muda tudo:

```'a'+ +'a'```

O operador `+` pode aparecer na forma unária pré-fixada ou na forma binária:

Unária: `+x`<br>
Binária: `x + y`

Na forma unária ele vai tentar converter o operando para um número:

```+'1' = 1```

Na forma binária ele vai somar os dois operandos, se ambos forem números, `1 + 3 // result 4`, e se um dos operandos for do tipo string, ele vai converter o outro operando para string e depois vai concatena-los:

```'1' + 3 // result '13'```

No caso do trecho `'a'+ +'a'`, tanto a operação unária como a operação binária estão sendo executadas.

Primeiro ele executara a operação unária que tem maior precedência `+'a'`, assim, vai tentar converter a string `'a'` para um número, resultando no valor `NaN`, porque a string `a` não um valor numérico válido.

Logo após isso, ele vai executar a operação binaria `'b' + NaN`, nesse caso como um dos operandos é uma string ele vai tentar converter o outro operando para uma string, nesse caso, o valor `NaN`, o que vai resultar em uma string `'NaN'`. Assim, quando executamos `'b'+'a'+ +'a'+'a'`, isso é oque acontece realmente:

```'b'+'a'+'NaN'+'a'```

Por fim, é só usar a função `toLocaleLowerCase` para deixar tudo com letras minúsculas, o que vai resultar na string final `'banana'`.

### Questão 2:

```javascript
let num = '10'

num += num++
```

**a) `'1010'`**

De inicio temos uma atribuição comum `let num = '10'`. Até esse ponto tudo normal, apenas criamos uma variável `num` e atribuimos a ela a string `'10'`. Seguindo no código temos:

```num += num++```

Aqui nós temos duas operações, uma operação unária de pós-incremento, e uma operação binária de atribuição com adição, o que vai definir nosso resultado final será a precedência desses operadores.

O operador de pós-incremento tem uma precedência de `17`, enquanto o operador de atribuição com adição tem uma precedência de `3`, aqui, quanto maior o valor da precedência, melhor. Sendo assim, nosso operador de pós-incremento ganhou de lavada.

Primeiro o nosso operador de pós-incremento ira tentar incrementar `1` a nossa variável `num++`. Porém essa variável é uma `string`, então ele tentar convertê-la para um valor numérico antes de somar mais `1` ao seu valor, nesse caso teremos o valor numérico `10`. Logo após fazer isso ele ira incrementar `1` a esse valor, gerando o valor numérico `11`, e por fim o operador iria retornar o valor `11`, certo? Errado!

A operação de pós-incremento, diferente da operação de pré-incremento, retorna o valor inicial do operando, no caso nossa string `'10'`.

Nesse ponto a operação de pós-incremento já foi finalizada, e só nos resta a operação de atribuição com adição. Essa operação primeiro ira utilizar o operador de adição e depois o operador de atribuição:

```num += num++```

Como o valor retornado da operação `num++` foi `'10'`, nosso operador de atribuição com adição vai tentar somar esse valor ao valor da variável `num` que também é `'10'`, resultando em uma concatenação de strings `'1010'`.

### Questão 3:

```javascript
(function () {
  let num1 = 10
  let num2 = 20

  return sum()

  function sum() {
    return num1 + num2
  }
})()
```

**c) `30`**

Nessa questão temos um caso clássico de <a href="https://developer.mozilla.org/pt-BR/docs/Glossario/Hoisting" target="_blank">Hoisting</a>, que o processo onde o interpretador JS pega todas as <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function" target="_blank">declarações de função</a> e as variáveis declaradas com `var`, e joga para o topo do escopo (o escopo global ou de um função).

No caso das variáveis, elas recebem o valor `undefined`, e continuam assim, ate que, explicitamente, recebam um valor. Já as <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function" target="_blank">declarações de função</a> tem um comportamento diferente, além de elevar o nome da função para o topo do escopo, o interpretador também elevará sua definição, portanto, podemos chamá-la antes de sua declaração.

E exatamente isso que acontece no código em questão. Quando a <a href="https://developer.mozilla.org/pt-BR/docs/Glossario/IIFE" target="_blank">IIFE</a> é executada, o interpretador JS pega todas as variáveis declaradas com `var` e todas as <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function" target="_blank">declarações de função</a>, e eleva para o topo do escopo da função. Nossa <a href="https://developer.mozilla.org/pt-BR/docs/Glossario/IIFE" target="_blank">IIFE</a> ficará com essa cara:

```javascript
(function () {
  let num1 = 10
  let num2 = 20
  function sum() {
    return num1 + num2
  }

  return sum()
})()
```

Assim, quando a função `sum` é executada, mesmo antes de sua declaração, ela retornará a soma dos valores da variáveis `num1` e `num2`.

### Questão 4:

```javascript
function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}

let person = Person('Guilherme', 'Moura')

console.log('log 1: ', person)
console.log('log 2: ', firstName, lastName)
```

**b) `log 1: undefined | log 2: Guilherme Moura`**

Aqui temos uma função construtora `Person`, que cria um objeto com as propriedades `firstName` e `lastName`. Após isso temos a seguinte a linha:

`let person = Person('Guilherme', 'Moura')`

Se não prestarmos atenção, podemos achar que esta linha esta criando um objeto com as propriedades `firstName = 'Guilherme'` e `lastName = 'Moura'`, e que este objeto esta sendo atribuido a variável `person`. Mas se observarmos atentamente essa operação, a função construtora esta sendo chamada sem a palavra chave `new`, e é aqui que as coisas mudam.

Por não estar sendo chamada com a palavra chave `new`, a função contrutora esta sendo executada normalmente, retornando um valor `undefined` por padrão. E esse valor que esta sendo atribuido a variável `person`.

Bem, isso responde o por que da variável `person` ser `undefined`, mas porque as variáveis `firstName` e `lastName` são acessiveis fora da função?

Em chamadas comuns de função, a variável `this` dentro da mesma sempre apontara para o objeto global `window`. Isso só deixa de ser verdade quando o código é executado com o `strict mode`, nesse caso, o `this` dentro das funções será `undefined`.

Como o `strict mode` não esta sendo usado, o `this` detro da função esta apontando para o `window`. É isso que realmente acontece quando a função `Person` é executada:

```javascript
function Person (firstName, lastName) {
  window.firstName = firstName
  window.lastName = lastName
}

// ...
```
São criadas duas variáveis globais: `firstName` e `lastName`. Podendo ser acessadas em qualquer parte do código.

### Questão 5:

```javascript
(3,5 - 3) * 2
```

**d) `4`**

Olhando rapidademente podemos pensar que a operação esta pegando `3.5` subtratindo `3` e multiplicando por `2`. Mas se olharmos atentamente, o que separa o `3` do `5` não é um ponto flutuante, mas sim, o <a href="https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/Operador_Virgula" target="_blank">operador de vírgula</a>. Esse operador avalia todos os seus operandos e retorna o ultimo deles.

Aqui existem dois operandos: o número `3` e o resultado da operação de subtração `5 - 3`. Será retornado o valor `2`, que é o resultado da subtração. Esse valor será multiplicado por `2` gerando o valor `4` como resultado final.

### Questão 6:

```javascript
let card = {};

(['audi','bmw'].map(item => card[item] = undefined), card);
```

**b)** `{ audi: undefined, bmw: undefined }`</br>

Uma das formas mais comuns de fazer atribuição dinâmica em um objeto javascript é através da notação de colchetes, dessa maneira é possível inserir valor a um atributo de forma dinâmica out até criar um atributo que ainda não existe no objeto.

Nesse caso mesmo identificando o valor como undefined o objeto irá marcar esses atributos como próprios do objeto ficando a mostra de forma explícita no objeto. Isso é bastante comum quando queremos que o objeto tenha um conjunto de propriedades.