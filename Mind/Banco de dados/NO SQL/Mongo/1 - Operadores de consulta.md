-------
Estes operadores são usados para filtrar documentos, principalmente dentro dos comandos `find()`, `findOne()` e no estágio `$match` do *pipeline* de agregação.

Nota: Sempre use `db.[collection]` antes dos comandos `find()`.
### Operadores Lógicos

Usados para combinar múltiplas condições.

```javascript
// $and: Todas as condições devem ser verdadeiras
db.usuarios.find( {
    $and: [
        { idade: { $gte: 18 } }, 
        { status: "ativo" }
    ] 
} )

// $or: Pelo menos UMA condição deve ser verdadeira
db.usuarios.find( {
    $or: [
        { nome: "Juliana" },
        { nome: "Fernanda" }
    ]
} )

// $not: Nega uma condição (inverte o resultado)
db.usuarios.find( { 
    preco: { $not: { $gt: 50 } } 
} )
// (Retorna documentos onde o preço NÃO é maior que 50)
```

- `$and`: E
- `$or`: OU
- `$not`: Negação

--------------
### Operadores de comparação

Usados para comparar valores de campos.

```javascript
// $eq: (Equal) Igual a
db.usuarios.find( { idade: { $eq: 25 } } )
// (Forma curta: db.usuarios.find( { idade: 25 } ))

// $ne: (Not Equal) Diferente de
db.usuarios.find( { status: { $ne: "inativo" } } )

// $gt: (Greater Than) Maior que
db.usuarios.find( { idade: { $gt: 18 } } )

// $gte: (Greater Than or Equal) Maior ou igual a
db.usuarios.find( { idade: { $gte: 18 } } )

// $lt: (Less Than) Menor que
db.usuarios.find( { media: { $lt: 7 } } )

// $lte: (Less Than or Equal) Menor ou igual a
db.usuarios.find( { media: { $lte: 7 } } )

// $in: (In) O valor está contido no array
db.usuarios.find( { categoria: { $in: ["eletrônico", "vestuário"] } } )

// $nin: (Not In) O valor NÃO está contido no array
db.usuarios.find( { categoria: { $nin: ["alimentício", "limpeza"] } } )
```

------
### Operadores de elemento

Usados para verificar a existência ou o tipo de um campo.

```javascript
// $exists: Verifica se o campo existe (ou não)
db.usuarios.find( { telefone: { $exists: true } } )
db.usuarios.find( { cupom: { $exists: false } } )

// $type: Verifica o tipo de dado BSON do campo
db.usuarios.find( { telefone: { $type: "string" } } )
db.usuarios.find( { idade: { $type: "int" } } )
// (Ver os tipos BSON no arquivo 01)
```

------
### Operadores de array (consulta)

Usados para filtrar com base em elementos de um array.

```javascript
// $all: O array deve conter TODOS os elementos especificados
db.usuarios.find( {
    seguros: { $all: ["seguro de vida", "seguro para carro"] }
} )

// $size: O array deve ter o tamanho (número de elementos) exato
db.usuarios.find( { hobbies: { $size: 3 } } )
```

---------
