------------
### Gerenciamento (Comandos Shell)

```bash
show dbs
# Lista todos os bancos de dados no servidor.

use [database]
# Troca para um banco de dados existente ou o cria (virtualmente) se ele não existir.

db
# Mostra o banco de dados atualmente em uso.

show collections
# Lista todas as collections (tabelas) no banco de dados atual.

db.dropDatabase()
# Apaga o banco de dados que você está usando atualmente.

db.[collection].renameCollection("[new_name]")
# Renomeia uma collection.
```

-------
### Tipos de Dados (BSON)

O MongoDB armazena documentos em BSON (Binary JSON). Estes são os tipos de dados principais e seus IDs numéricos:

|**Type**|**Number**|**Alias**|**Notes**|
|---|---|---|---|
|Double1|12|"double"3||
|String4|25|"string"6||
|Object7|38|"object"9||
|Array10|411|"array"12||
|Binary Data13|514|"binData"15||
|Undefined16|617|"undefined"18|Deprecated19|
|ObjectId20|721|"objectId"22||
|Boolean23|824|"bool"25||
|Date26|927|"date"28||
|Null29|1030|"null"31||
|Regular Expression3233|113435|"regex"3637||
|DBPointer3839|124041|"dbPoi42nter"43|Deprecated44|
|JavaScript45|1346|"javascript"47||
|Symbol48|1449|"symbol"50|Deprecated51|
|Javascript (with scope)52|1553|"javascriptWithScope"54||
|32-bit integer55|1656|"int"57||
|Timestamp58|1759|"timestamp"60||
|64-bit integer61|1862|"long"||
|Decimal128|19|"decimal"|New in Version 3.4|
|Min Key|-1|"minKey"||
|Max Key|127|"maxKey"||
|_(Fonte: Imagem 'Pasted image 20250821152838.png')_||||

-------
### CRUD (Create, Read, Update, Delete)

obs: Sempre use `db.[nome_da_collection]` antes dos comandos.
#### CREATE (Inserir)

```javascript
// Insere um único documento
db.usuarios.insertOne({ nome: "Ana", idade: 18, status: "ativo" })

// Insere múltiplos documentos (passando um array)
db.usuarios.insertMany([ 
    { nome: "Beto", idade: 25 }, 
    { nome: "Caio", idade: 32 } 
])
```

-----
#### READ (Ler / Consultar)

```javascript
// Retorna todos os documentos da collection (filtro vazio)
db.usuarios.find({})

// Retorna todos os documentos que correspondem ao filtro
db.usuarios.find({ status: "ativo" })

// Retorna apenas o PRIMEIRO documento que corresponde ao filtro
db.usuarios.findOne({ status: "ativo" })

// Busca um documento específico pelo seu _id (requer o objeto ObjectId)
db.usuarios.findById("6e8f8f8f8f8f8f8f8f8f8f8f") // (Nota: A sintaxe exata pode variar)

// Conta quantos documentos correspondem ao filtro
db.usuarios.countDocuments({ status: "ativo" })
```

------
#### UPDATE (Atualizar)

**Operador Principal:** `$set` (define o valor de um campo).

```javascript
// Atualiza o PRIMEIRO documento que corresponde ao filtro
db.usuarios.updateOne( 
    { nome: "Ana" },                       // Filtro (Query)
    { $set: { idade: 19 } }                // Ação de atualização
)

// Atualiza TODOS os documentos que correspondem ao filtro
db.usuarios.updateMany( 
    { status: "ativo" },                   // Filtro (Query)
    { $set: { ultima_atualizacao: new Date() } } // Ação de atualização
)
```

--------
#### DELETE (Deletar)

```javascript
// Apaga o PRIMEIRO documento que corresponde ao filtro
db.usuarios.deleteOne({ nome: "Beto" })

// Apaga TODOS os documentos que correspondem ao filtro
db.usuarios.deleteMany({ status: "inativo" })
```

-----
