-----------

Por padrão, o JavaScript é "single-threaded" (tem uma única linha de execução). Se uma tarefa demorada (como baixar uma imagem ou buscar dados de um servidor) fosse síncrona, ela "travaria" todo o resto, e a página ficaria sem responder.
O **Assincronismo** é o mecanismo que permite ao JavaScript iniciar uma tarefa demorada e continuar executando outros códigos. Quando a tarefa demorada termina, o JS é notificado e pode processar o resultado.
### Callbacks (O Jeito Antigo)

A forma original de lidar com assincronismo. Um *callback* é uma função que você passa como argumento para outra função, e ela é "chamada de volta" (executada) quando a operação termina.

O `setTimeout` é o exemplo mais simples:

```javascript
console.log("1. Tarefa iniciada.");

// Assíncrono: espera 2 segundos (2000ms) e DEPOIS executa o callback
setTimeout(() => {
    console.log("2. Tarefa demorada (Callback) terminou.");
}, 2000);

console.log("3. O resto do código continua executando...");

/*
Saída no console:
1. Tarefa iniciada.
2. O resto do código continua executando...
(espera 2 segundos)
3. Tarefa demorada (Callback) terminou.
*/
```

Problema: Múltiplas operações assíncronas aninhadas criam o "Callback Hell" (Inferno de Callbacks), um código difícil de ler e manter (o "Pyramid of Doom").

-----
### Promises (O Jeito Moderno - ES6)

Promises são objetos que representam a conclusão (ou falha) de uma operação assíncrona. Uma Promise pode estar em um de três estados:

- `pending`: (Pendente) Estado inicial, a operação ainda não terminou.
- `fulfilled`: (Resolvida) A operação foi concluída com sucesso.
- `rejected`: (Rejeitada) A operação falhou.
- 
Nós "consumimos" promises usando os métodos `.then()` (para sucesso) e `.catch()` (para falha).
#### Exemplo com `fetch()`

O `fetch()` é a API moderna do navegador para fazer requisições web (buscar dados em um servidor/API). Ele retorna uma Promise.

```javascript
console.log("1. Buscando dados...");

fetch('[https://api.github.com/users/google](https://api.github.com/users/google)')
    .then(resposta => {
        // .json() também é assíncrono e retorna uma promise
        return resposta.json();
    })
    .then(dados => {
        // Este .then() recebe o resultado do .then() anterior (os dados do usuário)
        console.log("2. Dados recebidos:", dados.name);
    })
    .catch(erro => {
        // .catch() é executado se qualquer parte da Promise falhar
        console.error("3. Houve um erro na busca:", erro);
    });

console.log("4. Código continua executando enquanto os dados são buscados...");
```

----------
### Async / Await (O Jeito Prático - ES8)

`async/await` é um "açúcar sintático" (uma forma mais bonita) de escrever código que usa Promises. Ele faz seu código assíncrono _parecer_ síncrono, tornando-o muito mais fácil de ler.

- `async`: Declara que uma função conterá operações assíncronas.
- `await`: **Pausa a execução da função** (e _apenas_ daquela função) até que a Promise seja resolvida ou rejeitada. Só pode ser usado dentro de uma `async function`.
- `try...catch`: É usado para tratar erros (substitui o `.catch()` da Promise).
#### Exemplo com `fetch()` reescrito:

```javascript
// A função precisa ser declarada como 'async'
async function buscarDadosDoGithub() {
    console.log("1. Iniciando a busca...");

    try {
        // 'await' pausa a função até o fetch terminar
        const resposta = await fetch('[https://api.github.com/users/google](https://api.github.com/users/google)');

        // 'await' pausa novamente até o .json() terminar
        const dados = await resposta.json();

        console.log("2. Dados recebidos:", dados.name);

    } catch (erro) {
        // Se qualquer 'await' falhar, o 'catch' é executado
        console.error("3. Houve um erro:", erro);
    }

    console.log("4. A função 'buscarDadosDoGithub' terminou.");
}

buscarDadosDoGithub();
console.log("5. Código fora da função async continua executando...");
```

-------
