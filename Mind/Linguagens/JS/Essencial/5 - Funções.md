--------------

Funções são blocos de código reutilizáveis. Elas permitem que você agrupe um conjunto de instruções para executar uma tarefa específica. Uma função só é executada quando é "chamada" ou "invocada".

### Declaração de Função (Function Declaration)

A forma tradicional de criar uma função. Ela é "elevada" (hoisted), o que significa que você pode chamá-la no código *antes* mesmo de ela ser declarada.

```javascript
// Declaração
function saudar(nome) {
    console.log("Olá, " + nome + "!");
}

// Chamada (Invocação)
saudar("Kear"); // Saída: Olá, Kear!
```

----
### Funções com Retorno (return)

Funções podem processar dados e "retornar" um valor. Quando a palavra-chave `return` é usada, a função para de executar e envia o valor de volta.

```javascript
function somar(a, b) {
    let resultado = a + b;
    return resultado;
}

let soma = somar(10, 5);
console.log(soma); // Saída: 15
```

--------
### Arrow Functions (ES6)

Uma sintaxe mais curta e moderna para escrever funções, introduzida no ES6. Elas são muito usadas hoje em dia, especialmente para funções curtas ou _callbacks_.

- Elas são anônimas (não têm um nome próprio, são geralmente atribuídas a variáveis).
- Elas têm um comportamento diferente com a palavra-chave `this` (um tópico mais avançado).

```javascript
// Sintaxe padrão da Arrow Function
const subtrair = (a, b) => {
    return a - b;
};

console.log(subtrair(20, 7)); // Saída: 13

// Sintaxe Curta (Retorno Implícito)
// Se a função tem APENAS UMA linha e essa linha é o "return",
// você pode remover as chaves {} e a palavra "return".

const multiplicar = (a, b) => a * b;

console.log(multiplicar(5, 5)); // Saída: 25

// Sintaxe com um único parâmetro
// Se a função tem APENAS UM parâmetro, os parênteses () são opcionais.

const dobrar = numero => numero * 2;

console.log(dobrar(10)); // Saída: 20
```

---------------------------------
