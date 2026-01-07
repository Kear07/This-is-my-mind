---------------------
Loops são usados para executar o mesmo bloco de código várias vezes, até que uma condição de parada seja atingida.
### For
O loop `for` é ideal quando você sabe *quantas vezes* quer que o código seja executado.

Ele é composto por três partes:
1.  **Inicialização:** Executa uma vez antes do loop começar (ex: `let i = 0`).
2.  **Condição:** Verificada *antes* de cada iteração. Se for `true`, o loop continua.
3.  **Incremento/Decremento:** Executa *ao final* de cada iteração (ex: `i++`).

```javascript
// Contagem de 0 até 4 (5 iterações)
for (let i = 0; i < 5; i++) {
    console.log("O número é: " + i);
}

// Iterando sobre um Array
let frutas = ["Maçã", "Banana", "Uva"];
for (let i = 0; i < frutas.length; i++) {
    console.log("Fruta:", frutas[i]);
}
```
(Nota: Existem formas mais modernas de iterar sobre arrays, como `.forEach()`, que veremos em "Estruturas de Dados")

----------
### While

O loop `while` (enquanto) é ideal quando você não sabe quantas vezes o loop precisa rodar, mas sabe a _condição de parada_.
O `while` verifica a condição e, se for `true`, executa o bloco. É crucial que algo _dentro_ do bloco altere a condição, senão você terá um loop infinito.

```javascript
let contador = 0;

while (contador < 5) {
    console.log("Contador: " + contador);
    contador++; // Se esquecer isso, o loop será infinito!
}
```

-------
### Do...while

Similar ao `while`, mas com uma diferença crucial: o bloco de código é executado **pelo menos uma vez**, e a condição é verificada _depois_ da execução.


```javascript
let i = 10;

do {
    console.log("Executou pelo menos uma vez. i = " + i);
    i++;
} while (i < 5); // A condição (10 < 5) é falsa, mas o bloco já rodou.
```

----------
### Controle de Loops: break e continue

- `break`: **Para** o loop imediatamente, saindo dele por completo.
- `continue`: **Pula** a iteração atual e vai para a próxima.

```javascript
for (let i = 0; i < 10; i++) {
    if (i === 3) {
        continue; // Pula a iteração onde i é 3
    }

    if (i === 7) {
        break; // Para o loop completamente quando i chega a 7
    }

    console.log(i);
}
// Saída: 0, 1, 2, 4, 5, 6
```

--------------

