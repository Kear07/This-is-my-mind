--------------------------

Strings são usadas para armazenar e manipular texto. Em JavaScript, elas são imutáveis (você não altera a string original, os métodos sempre retornam uma nova string).

### Formatação (Template Literals)

A forma mais moderna de formatar strings, equivalente às "f-strings" do Python. Usa-se a crase (`` ` ``) em vez de aspas.

```javascript
let nome = "Kear";
let idade = 25;

// Forma antiga (concatenação)
let saudacao1 = "Olá, meu nome é " + nome + " e eu tenho " + idade + " anos.";

// Forma moderna (Template Literal)
let saudacao2 = `Olá, meu nome é ${nome} e eu tenho ${idade} anos.`;
// Permite quebras de linha
let html = `
<div>
  <h1>${nome}</h1>
</div>
`;
```

---------------------------------------------------
### Acesso e Tamanho

- `.length`: (Propriedade) Retorna o número de caracteres na string.
- `[indice]`: Acessa um caractere específico (começando do índice 0).

```javascript
let texto = "JavaScript";

console.log(texto.length); // Saída: 10
console.log(texto[0]);     // Saída: "J"
console.log(texto[4]);     // Saída: "S"
```

----------
### Transformação (Maiúsculas/Minúsculas)

- `.toUpperCase()`: Converte toda a string para maiúsculas.
- `.toLowerCase()`: Converte toda a string para minúsculas.

```javascript
let msg = "Olá Mundo";
console.log(msg.toUpperCase()); // Saída: "OLÁ MUNDO"
console.log(msg.toLowerCase()); // Saída: "olá mundo"
```

------
### Limpeza (Espaços)

- `.trim()`: Remove espaços em branco do **início e do fim**.
- `.trimStart()`: Remove espaços em branco apenas do **início**.
- `.trimEnd()`: Remove espaços em branco apenas do **fim**.

```javascript
let email = "   contato@exemplo.com   ";
console.log(email.trim()); // Saída: "contato@exemplo.com"
```

-----
### Busca & Verificação

- `.includes(substring)`: Retorna `true` se a string contém a `substring`.
- `.indexOf(substring)`: Retorna o **índice** (posição) da primeira ocorrência da `substring`. Retorna `-1` se não encontrar.
- `.startsWith(substring)`: Retorna `true` se a string **começa** com a `substring`.
- `.endsWith(substring)`: Retorna `true` se a string **termina** com a `substring`.

```javascript
let frase = "Eu estou estudando JS.";

console.log(frase.includes("JS"));   // Saída: true
console.log(frase.includes("Python"));// Saída: false

console.log(frase.indexOf("estudando")); // Saída: 3
console.log(frase.indexOf("Java"));    // Saída: -1

console.log(frase.startsWith("Eu")); // Saída: true
console.log(frase.endsWith("JS."));  // Saída: true
```

------
### Substituição

```javascript
let texto = "gato preto, gato branco";

// Substitui apenas o primeiro "gato"
console.log(texto.replace("gato", "cachorro"));
// Saída: "cachorro preto, gato branco"

// Substitui todos os "gato"
console.log(texto.replaceAll("gato", "cachorro"));
// Saída: "cachorro preto, cachorro branco"
```

----
### Extração (Fatiamento)

- `.slice(inicio, fim)`: Extrai um pedaço da string, do índice `inicio` até o `fim` (não incluso). 
    - Se o `fim` for omitido, vai até o final.
    - Aceita índices negativos (começa a contar do final).
- `.substring(inicio, fim)`: Similar ao `.slice()`, mas não aceita índices negativos.

```javascript
let str = "JavaScript";

// Pega de "Java"
console.log(str.slice(0, 4)); // Saída: "Java"

// Pega de "Script" (do índice 4 até o fim)
console.log(str.slice(4)); // Saída: "Script"

// Pega os últimos 3 caracteres
console.log(str.slice(-3)); // Saída: "ipt"
```

-----------
### Divisão

- `.split(separador)`: Divide a string em um **array** de substrings, usando o `separador` como ponto de divisão.

```javascript
let csv = "maçã;banana;uva;laranja";

let listaFrutas = csv.split(";");
// Saída: ["maçã", "banana", "uva", "laranja"]

let palavra = "Kear";
let letras = palavra.split("");
// Saída: ["K", "e", "a", "r"]
```
(O método `.join()` para juntar o array de volta em uma string será visto em "Estruturas de Dados")
