-----------
### Declaração de Variáveis

Existem três formas principais de declarar variáveis em Java Script:

* `var`:    Forma antiga de declarar variáveis. **Evite usar**, pois pode ter comportamentos inesperados (escopo de função).
* `let`:    Forma moderna. Permite que o valor da variável seja reatribuído. Tem escopo de bloco (tudo que está entre `{}`).
* `const`:  Forma moderna. A variável **não pode ser reatribuída** após sua inicialização. Também tem escopo de bloco.

**Importante sobre `const`:** Se a `const` armazena um array ou um objeto, você *pode* alterar o *conteúdo* interno (elementos do array ou propriedades do objeto), mas não pode fazer a variável apontar para um novo array ou objeto.

```javascript
// let (valor pode mudar)
let idade = 30;
idade = 31; // Válido

// const (valor não pode ser reatribuído)
const nome = "João";
// nome = "Maria"; // Inválido! Gera um erro.

// const com objeto (conteúdo interno pode mudar)
const pessoa = { tipo: "Humano" };
pessoa.tipo = "Dinossauro"; // Válido!
```

-----------------
### Tipos de Dados Primitivos

- **String:** Sequência de caracteres (texto).

```javascript
let nome = "Kear";
let frase = 'JavaScript é legal';
let template = `Olá, ${nome}!`; // Template Literal
```

- Number: Para números inteiros ou de ponto flutuante (decimal).

```javascript
let idade = 25;
let preco = 19.99;
```

- Boolean: Valor lógico, verdadeiro (`true`) ou falso (`false`).

```javascript
let programador = true;
let admin = false;
```

- Undefined: Uma variável que foi declarada, mas ainda não teve um valor atribuído.

```javascript
let carro;
console.log(carro); // Saída: undefined
```

 - null: Representa a ausência intencional de um valor de objeto.

```javascript
let usuario = null;
```

---------
### Operadores aritméticos

- `+`: Adição
- `-`: Subtração
- `*`: Multiplicação
- `/`: Divisão
- `%`: Módulo (Resto da divisão)
- `**`: Exponenciação (Ex: `2 ** 3` = 8)

-------------
### Atribuição

- `=`: Atribuição simples
- `+=`: Adição e atribuição (`x += y` é o mesmo que `x = x + y`)
- `-=`: Subtração e atribuição
- `*=`: Multiplicação e atribuição
- `/=`: Divisão e atribuição
- `%=`: Módulo e atribuição

--------
### Comparação

- `==`: Igualdade (compara apenas o valor, faz coerção de tipo - **Evitar**)
- `!=`: Diferença (compara apenas o valor - **Evitar**)
- `===`: Igualdade Estrita (compara valor **e** tipo - **Recomendado**)
- `!==`: Diferença Estrita (compara valor **e** tipo - **Recomendado**)
- `>`: Maior que
- `<`: Menor que
- `>=`: Maior ou igual a
- `<=`: Menor ou igual a

-----------------
### Lógicos

- `&&`: **E** (Ambas as condições devem ser verdadeiras)
- `||`: **OU** (Pelo menos uma condição deve ser verdadeira)  
- `!`: **NÃO** (Inverte o valor lógico)

-----------
### Operador Ternário

Uma forma curta de escrever um `if/else` simples.

- Sintaxe: `Condição ? valor_se_verdadeiro : valor_se_falso`

```javascript
let idade = 20;
let resultado = (idade >= 18) ? "Maior de idade" : "Menor de idade";
// resultado será "Maior de idade"
```

-----------
### Saída (Output)

Para exibir informações no console do navegador ou terminal.

```javascript
console.log("Olá, mundo!");
console.log("Valor da variável:", idade);
```
