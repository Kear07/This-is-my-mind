------------------
### Arrays (Listas)

Arrays são coleções ordenadas de itens. Eles podem guardar qualquer tipo de dado (números, strings, objetos, etc.) e são mutáveis.

```javascript
// Criação
let frutas = ["Maçã", "Banana", "Laranja"];
let numeros = [1, 2, 3, 4, 5];
let misto = [1, "Texto", true];
```

**Acessar Itens:** Pelo índice (começa em 0).

```javascript
console.log(frutas[0]); // Saída: "Maçã"
console.log(frutas[1]); // Saída: "Banana"
```

Tamanho do Array:

```javascript
console.log(frutas.length); // Saída: 3
```

Modificar Itens:

```javascript
frutas[1] = "Uva";
// frutas agora é ["Maçã", "Uva", "Laranja"]
```
-----------
### Métodos de Modificação (Principais)

- `.push(item)`: Adiciona um item no **final** do array.
- `.pop()`: Remove o **último** item do array.
- `.unshift(item)`: Adiciona um item no **início** do array.
- `.shift()`: Remove o **primeiro** item do array.
- `.splice(indice, deleteCount, ...items)`: O método "canivete suíço". Pode remover, substituir ou adicionar itens em qualquer posição.

```javascript
let lista = ["A", "B", "C"];

lista.push("D");      // lista = ["A", "B", "C", "D"]
lista.pop();          // lista = ["A", "B", "C"]
lista.unshift("Z");   // lista = ["Z", "A", "B", "C"]
lista.shift();        // lista = ["A", "B", "C"]

// Remove 1 item a partir do índice 1 ("B")
lista.splice(1, 1);   // lista = ["A", "C"]
```
--------
### Métodos de Iteração (Modernos - Essenciais)

Estes métodos **não modificam** o array original (exceto o `forEach`), mas sim retornam novos valores ou novos arrays.

- `.forEach(callback)`: Executa uma função para **cada item** do array. (Não retorna nada).

```javascript
frutas.forEach((fruta) => {
    console.log("Eu gosto de " + fruta);
});
```

- `.map(callback)`: **Cria um novo array** transformando cada item do array original.

```javascript
let numeros = [1, 2, 3];
let dobrados = numeros.map((num) => {
    return num * 2;
});
// dobrados = [2, 4, 6]
```

- `.filter(callback)`: **Cria um novo array** apenas com os itens que passam em uma condição (callback deve retornar `true` ou `false`).

```javascript
let numeros = [1, 2, 3, 4, 5, 6];
let pares = numeros.filter((num) => {
    return num % 2 === 0;
});
// pares = [2, 4, 6]
```

- `.find(callback)`: Retorna o **primeiro item** que satisfaz a condição.

```javascript
let pessoas = [{id: 1, nome: "Ana"}, {id: 2, nome: "João"}];
let joao = pessoas.find((p) => p.nome === "João");
// joao = {id: 2, nome: "João"}
```
----------------
### Objetos (JSON)

Objetos são coleções **não ordenadas** de pares "chave-valor". São a principal forma de estruturar dados complexos em JS (equivalente aos dicionários em Python).

```javascript
// Criação
let pessoa = {
    nome: "Kear",
    idade: 25,
    programador: true,
    "cidade-natal": "São Paulo" // Chaves com caracteres especiais usam aspas
};
```

#### Acesso de Propriedades

- **Dot Notation (Notação de Ponto):** Mais comum e fácil.

```javascript
console.log(pessoa.nome); // Saída: "Kear"
console.log(pessoa.idade); // Saída: 25
```

- **Bracket Notation (Notação de Colchetes):** Necessária para chaves dinâmicas (variáveis) ou chaves com caracteres especiais.

```javascript
console.log(pessoa["programador"]); // Saída: true
console.log(pessoa["cidade-natal"]); // Saída: "São Paulo"

let chave = "idade";
console.log(pessoa[chave]); // Saída: 25
```

#### Modificação, Adição e Remoção

- Modificando:

```javascript
pessoa.idade = 26;
```

- Adicionando:

```javascript
pessoa.email = "kear@email.com";
```

- Removendo:

```javascript
delete pessoa.programador;
```

----------------