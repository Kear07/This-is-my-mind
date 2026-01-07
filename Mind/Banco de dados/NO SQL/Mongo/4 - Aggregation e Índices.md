-------------
### Índices (Indexes)

Índices são estruturas de dados especiais que armazenam um pequeno subconjunto de dados da *collection* de uma forma fácil de atravessar. Eles melhoram drasticamente a velocidade das consultas (`find`).

```javascript
// Cria um índice ascendente (1) no campo 'idade'.
// Use -1 para descendente.
db.usuarios.createIndex( {idade: 1} )

// Cria um índice composto (ordena primeiro por 'categoria', depois por 'preco')
db.usuarios.createIndex( {categoria: 1, preco: -1} )

// Lista todos os índices da collection
db.usuarios.getIndexes()

// Apaga um índice específico (você pode ver o nome com getIndexes())
db.usuarios.dropIndex("nome_do_indice")
```

------
### Aggregation Pipeline

O _Aggregation Pipeline_ (Funil de Agregação) é um framework para processar dados em múltiplos estágios. Os documentos entram no início e passam por cada estágio, sendo transformados, filtrados ou agrupados.

**Nota:** Sempre use `db.[collection].aggregate([ ... ])`
#### Estágios comuns do pipeline

```javascript
// $match: Filtra os documentos. (Usa os mesmos operadores do 'find')
{ $match: {status: "ativo", idade: {$gte: 18 }} }

// $group: Agrupa documentos por um campo (id) e aplica acumuladores.
{ $group: {_id: "$categoria", total: {$sum: "$preco" }} }

// $project: Seleciona, exclui ou cria novos campos.
// (1 para incluir, 0 para excluir)
{ $project: {nome: 1, idade: 1, _id: 0 } }

// $sort: Ordena os documentos.
// (-1 para decrescente, 1 para crescente)
{ $sort: {idade: -1 } }

// $limit: Limita o número de documentos para o próximo estágio.
{ $limit: 10 }

// $skip: Pula os 'n' primeiros documentos.
{ $skip: 5 }

// $unwind: Desconstrói um campo array.
// (Cria um documento de saída para CADA elemento do array).
{ $unwind: "$tags" }

// $lookup: Faz um "JOIN" com outra collection.
{ $lookup: {
    from: "outra_colecao",     // Collection para "juntar"
    localField: "id_local",    // Campo na collection atual
    foreignField: "id_fk",     // Campo na 'outra_colecao'
    as: "novo_campo_array"   // Nome do novo campo (será um array)
}}

// $count: Conta o número de documentos no estágio e atribui a um novo campo.
{ $count: "totalDeDocumentos" }
```

---------
### Operadores de Expressão (Usados em `$group`, `$project`, etc.)

#### Acumuladores (Comuns em `$group`)

```javascript
// $sum: Soma valores.
{ totalVendas: {$sum: "$preco" } }

// $avg: Calcula a média.
{ mediaIdade: {$avg: "$idade" } }

// $max: Valor máximo.
{ precoMaximo: {$max: "$preco" } }

// $min: Valor mínimo.
{ precoMinimo: {$min: "$preco" } }

// $count: Conta o número de documentos no grupo.
{ quantidadeVendas: { $count: {} } }
```

-----
#### Operadores Matemáticos (Comuns em `$project`)

```javascript
// $multiply: Multiplica valores.
{ faturamento: {$multiply: [ "$preco", "$quantidade" ] } }

// $subtract: Subtrai valores.
{ precoFinal: {$subtract: [ "$preco", "$desconto" ] } }

// $divide: Divide valores.
{ media: {$divide: [ "$total", "$itens" ] } }

// $round: Arredonda um número.
{ precoAprox: {$round: [ "$preco", 2 ] } } // Arredonda para 2 casas decimais

// $trunc: Remove as casas decimais.
{ preco_truncado: { $trunc: "$preco" } }
```
#### Operadores de String (Comuns em `$project`)

- `$split`: Divide uma string em array com base em um delimitador.    
- `$concat`: Junta várias strings em uma só.
- `$toLower`: Converte string para minúsculas.
- `$toUpper`: Converte string para maiúsculas.
- `$trim`: Remove espaços (ou caracteres especificados) do início e fim.
- `$ltrim`: Remove apenas da esquerda.
- `$rtrim`: Remove apenas da direita.
- `$replaceOne`: Substitui a primeira ocorrência de um texto.
- `$replaceAll`: Substitui todas as ocorrências de um texto.
- `$substrCP`: Extrai parte da string (recomendado p/ Unicode).

-----------
