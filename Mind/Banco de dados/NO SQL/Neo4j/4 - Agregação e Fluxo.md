----------------------------------------------------
### `WITH` (O "Pipe" do Cypher)

O `WITH` é, sem dúvida, o comando mais importante para consultas complexas. Ele funciona como o "pipe" (`|`) no Shell ou um "return" no meio da consulta.

Ele permite que você **passe os resultados** de uma parte da consulta (ex: um `MATCH`) para a próxima, permitindo filtrar, agregar e transformar os dados em etapas.

**Uso principal:** Filtrar agregações (o `HAVING` do SQL).

Em SQL, você faria:
`... GROUP BY p.nome HAVING count(prod) > 5`

Em Cypher, você faz:
```cypher
// 1. Encontra o padrão
MATCH (p:Pessoa)-[:COMPROU]->(prod:Produto)

// 2. Agrega os resultados com WITH
WITH p, count(prod) AS totalDeProdutos

// 3. Filtra a agregação com WHERE
WHERE totalDeProdutos > 5

// 4. Retorna o resultado final
RETURN p.nome, totalDeProdutos
```

---------
### Agregação (Aggregation)

São funções que agrupam resultados. Elas são usadas com `WITH` ou `RETURN`.

```cypher
// count(): Conta o número de ocorrências
// Quantas pessoas existem no banco?
MATCH (p:Pessoa)
RETURN count(p)

// collect(): Agrupa valores em uma LISTA (muito poderoso)
// Para cada pessoa, colete uma lista dos produtos que ela comprou
MATCH (p:Pessoa)-[:COMPROU]->(prod:Produto)
RETURN p.nome, collect(prod.nome) AS produtosComprados
// Saída: "Ana", ["Notebook", "Mouse", "Teclado"]

// Funções matemáticas
MATCH (prod:Produto)
RETURN avg(prod.preco) AS mediaPreco,
       sum(prod.estoque) AS estoqueTotal,
       min(prod.preco) AS maisBarato,
       max(prod.preco) AS maisCaro
```

---------
### `UNWIND` (O Oposto de `collect`)

O `UNWIND` pega uma **lista** (seja um array literal ou uma propriedade de um nó) e a "desenrola", transformando cada item da lista em uma **linha** separada.

É o comando essencial para **importar dados** (ex: de um JSON).

```cypher
// 1. Começa com uma lista
WITH ["Maçã", "Banana", "Laranja"] AS frutas

// 2. Desenrola a lista (cria 3 linhas)
UNWIND frutas AS fruta

// 3. Cria um nó para cada linha
CREATE (f:Fruta {nome: fruta})
// (Resultado: 3 nós :Fruta foram criados)
```

-------------
### Ordenação e Paginação

Idêntico ao SQL, usado no final da consulta.

- `ORDER BY`: Ordena os resultados.  
- `SKIP`: Pula um número de resultados.
- `LIMIT`: Limita o número de resultados.

```cypher
// Encontra os 10 produtos mais caros,
// mas pula os 5 primeiros (mostra do 6º ao 10º)
MATCH (p:Produto)
RETURN p.nome, p.preco
ORDER BY p.preco DESC
SKIP 5
LIMIT 10
```

-----------
### Lógica Condicional (CASE)

Permite usar lógica `if/else` dentro do seu `RETURN` ou `SET` para transformar dados.

```cypher
MATCH (p:Pessoa)
RETURN p.nome,
CASE
    WHEN p.idade >= 18 THEN "Adulto"
    WHEN p.idade < 18 AND p.idade >= 13 THEN "Adolescente"
    ELSE "Criança"
END AS faixaEtaria
```

-------
