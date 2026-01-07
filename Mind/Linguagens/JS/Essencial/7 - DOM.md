--------------------

O **DOM** (Document Object Model) é a representação em árvore do seu arquivo HTML. O JavaScript usa o DOM para "ver" e "modificar" o HTML, permitindo alterar conteúdo, estilos e estrutura da página dinamicamente.

### Seleção de Elementos (Querying) (Modo moderno)

Para manipular um elemento, você primeiro precisa selecioná-lo.
Usam seletores CSS (os mesmos que você usa no seu arquivo `.css`).

* `.querySelector(seletor)`: Retorna o **primeiro** elemento que corresponde ao seletor.
* `.querySelectorAll(seletor)`: Retorna uma **NodeList** (similar a um array) com **todos** os elementos que correspondem ao seletor.

```javascript
// Seleciona o primeiro elemento com o ID 'titulo'
let titulo = document.querySelector("#titulo");

// Seleciona o primeiro elemento com a classe 'item-lista'
let item = document.querySelector(".item-lista");

// Seleciona a primeira tag <p>
let paragrafo = document.querySelector("p");

// Seleciona TODOS os elementos com a classe 'item-lista'
let todosItens = document.querySelectorAll(".item-lista");

// Você pode iterar sobre a NodeList com forEach
todosItens.forEach((elemento) => {
    console.log(elemento);
});
```

-------
### Seleção de Elementos (Querying) (Modo ### clássico)

- `.getElementById(id)`: Seleciona UM elemento pelo seu `id` (sem o `#`).
- `.getElementsByClassName(classe)`: Retorna uma `HTMLCollection` (parecido com array) de elementos pela `class` (sem o `.`).
- `.getElementsByTagName(tag)`: Retorna uma `HTMLCollection` pela `tag` (ex: "p", "div").

```javascript
let titulo = document.getElementById("titulo");
let itens = document.getElementsByClassName("item-lista");
```

--------------
### Manipulação de Conteúdo

Depois de selecionar um elemento, você pode mudar o que está dentro dele.

- `.innerHTML = "novo <b>HTML</b>";`
    - Define o conteúdo HTML _interno_ de um elemento. Interpreta as tags HTML.
    - **Cuidado:** Pode ser inseguro se usado com dados do usuário (risco de XSS).
- `.textContent = "novo texto";`
    - Define o conteúdo de _texto_ de um elemento. Não interpreta tags HTML (trata tudo como texto puro).
    - **Recomendado** para inserir apenas texto (mais seguro e rápido).

```javascript
let titulo = document.querySelector("#titulo");

titulo.textContent = "Novo Título (Texto)";
// <h1 id="titulo">Novo Título (Texto)</h1>

titulo.innerHTML = "Novo <i>Título</i> (HTML)";
// <h1 id="titulo">Novo <i>Título</i> (HTML)</h1>
```

-------

### Manipulação de Estilos (CSS)

#### Estilo Inline (elemento.style)

Permite alterar o CSS _inline_ (o atributo `style=""` no HTML).
**Importante:** Propriedades CSS com hífen (ex: `background-color`) são escritas em `camelCase` no JavaScript (ex: `backgroundColor`).

```javascript
let elemento = document.querySelector("#meu-bloco");

elemento.style.color = "red";
elemento.style.backgroundColor = "#FFF"; // background-color
elemento.style.fontSize = "24px";        // font-size
```

#### Classes (elemento.classList) - Recomendado

A forma mais organizada de alterar estilos é manipulando as classes CSS do elemento.

- `.classList.add("nome-da-classe")`: Adiciona uma classe.
- `.classList.remove("nome-da-classe")`: Remove uma classe.
- `.classList.toggle("nome-da-classe")`: Adiciona se não tiver, remove se tiver.

```javascript
/* No seu CSS */
.ativo {
    background-color: blue;
    font-weight: bold;
}
```

```javascript
/* No seu JS */
let botao = document.querySelector("#meu-botao");

// Adiciona a classe .ativo ao botão
botao.classList.add("ativo");

// Remove a classe .ativo
botao.classList.remove("ativo");

// Alterna a classe (ótimo para cliques)
botao.classList.toggle("ativo");
```

-----------
### Manipulação de Atributos

Permite ler ou definir atributos HTML (como `src` de uma imagem, `href` de um link, `value` de um input).

- `.setAttribute("atributo", "valor")`: Define ou altera um atributo.
- `.getAttribute("atributo")`: Lê o valor de um atributo.

```javascript
let link = document.querySelector("#meu-link");
let imagem = document.querySelector("#minha-img");

// Altera o destino do link
link.setAttribute("href", "[https://www.google.com](https://www.google.com)");

// Altera a imagem exibida
imagem.setAttribute("src", "nova-imagem.png");

// Lê o valor do atributo
console.log(link.getAttribute("href")); // Saída: [https://www.google.com](https://www.google.com)
```

---------
