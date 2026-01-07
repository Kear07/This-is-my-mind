-----------------

Eventos são ações ou ocorrências que acontecem no navegador, como um clique de mouse, o pressionar de uma tecla, ou o carregamento de uma página. JavaScript nos permite "ouvir" (listen) esses eventos e executar uma função (um *callback*) quando eles ocorrem.

### O Método Moderno: `addEventListener()`

Esta é a forma **recomendada** de lidar com eventos. Ela permite adicionar múltiplas funções "ouvintes" para o mesmo evento em um mesmo elemento.

* **Sintaxe:** `elemento.addEventListener("tipo_do_evento", funcao_callback);`

* `elemento`: O elemento HTML que você selecionou (ex: com `querySelector`).
* `tipo_do_evento`: O nome do evento em string (ex: `"click"`, `"keydown"`).
* `funcao_callback`: A função que será executada *quando* o evento acontecer.

#### Exemplo de Uso

```html
<button id="meu-botao">Clique em mim!</button>
```

```javascript
// No seu JS
// 1. Seleciona o elemento
let botao = document.querySelector("#meu-botao");

// 2. Define a função que será executada
function aoClicar() {
    console.log("O botão foi clicado!");
    botao.textContent = "Clicado!";
}

// 3. Adiciona o "ouvinte" de evento
botao.addEventListener("click", aoClicar);
```

#### Exemplo com Arrow Function Anônima

É muito comum definir a função _diretamente_ dentro do `addEventListener` usando uma Arrow Function:

```javascript
let botao = document.querySelector("#meu-botao");

botao.addEventListener("mouseover", () => {
    console.log("O mouse está sobre o botão!");
});
```

------------
### O Objeto `event`

A função de callback recebe automaticamente um parâmetro (geralmente chamado de `event` ou `e`) que contém informações sobre o evento que ocorreu.

```javascript
let input = document.querySelector("#meu-input");

input.addEventListener("keydown", (event) => {
    // event.key contém a tecla que foi pressionada
    console.log("Tecla pressionada:", event.key);
});
```

- `event.preventDefault()`

Um uso essencial do objeto `event` é o `.preventDefault()`. Ele impede o comportamento padrão do navegador. É mais usado em eventos de `submit` de formulários (para impedir que a página recarregue).

```javascript
let formulario = document.querySelector("#meu-form");

formulario.addEventListener("submit", (event) => {
    // Impede o formulário de recarregar a página
    event.preventDefault();
    console.log("Formulário enviado (mas a página não recarregou)!");
    // Aqui você faria a validação ou envio dos dados com JS
});
```

------------------------
### Lista de Eventos Comuns

- **Eventos de Mouse:**   
    - `click`: Disparado quando recebe um clique.
    - `dblclick`: Disparado quando recebe um clique duplo.
    - `mouseover`: Disparado quando o ponteiro do mouse entra no elemento.
    - `mouseout`: Disparado quando o ponteiro do mouse sai do elemento.
    - `mousedown`: Disparado quando o botão do mouse é pressionado.
    - `mouseup`: Disparado quando o botão do mouse é solto.
    
- **Eventos de Teclado:**
    - `keydown`: Disparado quando uma tecla é **pressionada**.
    - `keyup`: Disparado quando uma tecla é **solta**.
    
- **Eventos de Formulário (Inputs, Selects, etc.):**
    - `focus`: Disparado quando o elemento recebe o foco (ex: ao clicar num input).
    - `blur`: Disparado quando o elemento perde o foco.
    - `change`: Disparado quando o valor de um elemento muda (ex: após selecionar algo num `<select>`).
    - `submit`: Disparado quando um formulário (`<form>`) é enviado.
        
- **Eventos de Página (Janela):**
    - `load`: Disparado quando a página (ou um recurso, como imagem) terminou de carregar. (Usado no `window` ou `body`).
    - `resize`: Disparado quando a janela do navegador é redimensionada.

----
