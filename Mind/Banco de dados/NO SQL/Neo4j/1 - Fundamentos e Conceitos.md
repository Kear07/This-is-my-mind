---------
### O que é um Banco de Dados de Grafo?

Enquanto bancos SQL são otimizados para agregar dados em tabelas (ex: `SUM()`, `GROUP BY`), e bancos de Documento são otimizados para armazenar estruturas flexíveis (JSON), o Neo4j é otimizado para uma coisa: **relações**.

Ele foi projetado desde o início para armazenar e consultar dados de forma "conectada", tornando-o ideal para responder perguntas como:
* "Quais amigos dos meus amigos também compraram este produto?"
* "Qual o caminho mais curto entre o Ponto A e o Ponto B nesta rede logística?"
* "Este usuário faz parte de um anel de fraude conhecido?"

Tentar responder isso em SQL exigiria `JOIN`s complexos e lentos. No Neo4j, é uma consulta trivial.

--------------
### A Tríade: O Modelo de Grafo de Propriedade

O Neo4j usa o "Labeled Property Graph model". Tudo se resume a três blocos de construção:

#### a) Nós (Nodes)
* **O que são:** As entidades, os "substantivos" do seu domínio.
* **Exemplos:** Uma `Pessoa`, um `Produto`, uma `Empresa`, uma `Cidade`.
* **Em Cypher (linguagem):** São representados por parênteses `()`.

#### b) Rótulos (Labels)
* **O que são:** As "etiquetas" que definem o tipo de um Nó (como o nome de uma tabela em SQL).
* **Exemplos:** `:Pessoa`, `:Produto`, `:Filme`.
* **Regras:** Um Nó pode ter *zero ou múltiplos* Rótulos. Isso é muito poderoso. Um nó pode ser `(n:Pessoa:Cliente:VIP)`.
* **Em Cypher:** `(n:Pessoa)`

#### c) Relações (Relationships)
* **O que são:** A parte mais importante. São os "verbos" que conectam os Nós e dão significado à conexão.
* **Exemplos:** `[:AMIGO_DE]`, `[:COMPROU]`, `[:ATUOU_EM]`, `[:NASCEU_EM]`.
* **Regras:**
    1.  Relações **sempre** têm uma direção (indicada pela seta `->`).
    2.  Relações **sempre** têm um tipo (o nome, ex: `:COMPROU`).
    3.  Relações conectam *exatamente dois* Nós.
* **Em Cypher:** São representadas por colchetes `[]` e setas `-->` ou `<--`.
* **Exemplo:** `(kear:Pessoa) -[:AMIGO_DE]-> (ana:Pessoa)`

#### d) Propriedades (Properties)
* **O que são:** Pares de chave-valor (JSON) que armazenam os dados.
* **Onde vivem:** Tanto os **Nós** quanto as **Relações** podem ter propriedades.
* **Exemplos:**
    * Nó `(p:Pessoa {nome: "Kear", idade: 25})`
    * Relação `-[r:COMPROU {data: "2025-11-05", valor: 199.99}]->`

--------
### O "ASCII Art" do Cypher

Cypher é a linguagem de consulta do Neo4j. Ela foi projetada para ser visual, como um "ASCII art" dos seus dados.

* `()` -> Um Nó
* `(:Pessoa)` -> Um Nó com o Rótulo "Pessoa"
* `(kear)` -> Um Nó atribuído à variável "kear"
* `(kear:Pessoa)` -> Um Nó com Rótulo "Pessoa", atribuído à variável "kear"
* `(kear:Pessoa {nome: "Kear"})` -> Um Nó com Rótulo e Propriedade

* `[]` -> Uma Relação
* `[:AMIGO_DE]` -> Uma Relação com o Tipo "AMIGO_DE"
* `[r:AMIGO_DE]` -> Uma Relação do Tipo "AMIGO_DE", atribuída à variável "r"

**Juntando tudo:**
Este padrão Cypher...
```cypher
(p:Pessoa {nome: "Ana"}) -[r:COMPROU]-> (prod:Produto {nome: "Notebook"})
```

...significa literalmente: "Encontre um Nó com Rótulo `Pessoa` cuja propriedade `nome` é 'Ana', que tenha uma Relação do tipo `COMPROU` apontando para um Nó com Rótulo `Produto` cuja propriedade `nome` é 'Notebook'."

--------
