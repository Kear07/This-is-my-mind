---------------------
Estes operadores são usados no segundo argumento dos comandos `updateOne()`, `updateMany()` e `findOneAndUpdate()` para modificar os dados do documento.
**Nota:** Sempre use `db.[collection]` antes dos comandos.
### Operadores de campo

Modificam valores de campos específicos.

```javascript
// $set: Define (ou adiciona) o valor de um campo.
db.usuarios.updateOne( 
    {_id: 1}, 
    { $set: {nome: "Carlos", status: "ativo"} } 
)

// $unset: Remove um campo específico do documento.
db.usuarios.updateOne( 
    {_id: 1}, 
    { $unset: {telefone: ""} } // O valor "" é irrelevante
)

// $rename: Renomeia um campo.
db.usuarios.updateMany( 
    {}, 
    { $rename: {"endereço": "endereco"} } 
)

// $inc: Incrementa (ou decrementa) um valor numérico.
db.usuarios.updateOne( 
    {_id: 1 }, 
    { $inc: {saldo: 100, bonus: -10} } // Adiciona 100 ao saldo, subtrai 10 do bônus
)

// $mul: Multiplica um valor numérico.
db.usuarios.updateOne( 
    {_id: 1}, 
    { $mul: {saldo: 1.1} } // Aumenta o saldo em 10%
)

// $min: Atualiza o campo APENAS se o novo valor for MENOR que o atual.
db.usuarios.updateOne( 
    {_id: 1}, 
    { $min: {salario: 5000} } // Se o salário for 6000, muda para 5000. Se for 4000, não muda.
)

// $max: Atualiza o campo APENAS se o novo valor for MAIOR que o atual.
db.usuarios.updateOne( 
    {_id: 1 }, 
    { $max: {pontuacao: 1000 } } // Se a pontuação for 800, muda para 1000. Se for 1200, não muda.
)

// $setOnInsert: Define um valor APENAS se a operação for um "upsert" (update com upsert:true) e resultar em uma INSERÇÃO.
db.usuarios.updateOne(
    {nome: "NovoUsuario"},
    { 
        $set: {ultimo_login: new Date()},
        $setOnInsert: { data_criacao: new Date() } 
    },
    { upsert: true }
)
```

----------
### Operadores de array

Modificam campos que são arrays.

```javascript
// $push: Adiciona um elemento ao final do array.
db.usuarios.updateOne( 
    {_id: 1}, 
    { $push: {hobbies: "leitura"} } 
)

// $push com $each: Adiciona MÚLTIPLOS elementos ao array.
db.usuarios.updateOne( 
    {_id: 1}, 
    { $push: {scores: { $each: [ 1, 3, 5 ] }} }
)

// $push com $slice: Limita o tamanho do array após adicionar (ex: manter só os 5 últimos)
db.usuarios.updateOne(
    {_id: 1},
    { $push: { logs: { $each: ["novo log"], $slice: -5 } } }
)

// $addToSet: Adiciona um elemento ao array APENAS se ele ainda não existir.
db.usuarios.updateOne( 
    {_id: 1}, 
    { $addToSet: {hobbies: "leitura"} } // Ignorado se "leitura" já existir
)

// $pop: Remove o primeiro (-1) ou o último (1) elemento do array.
db.usuarios.updateOne( 
    {_id: 1}, 
    { $pop: {scores: 1} } // Remove o último item
)

// $pull: Remove TODAS as ocorrências de um valor específico do array.
db.usuarios.updateOne( 
    {_id: 1 }, 
    { $pull: {hobbies: "pesca"} } 
)

// $pullAll: Remove todas as ocorrências de MÚLTIPLOS valores do array.
db.usuarios.updateOne( 
    {_id: 1}, 
    { $pullAll: {scores: [ 0, 5 ] } } 
)

// $pull (com $in): Alternativa ao $pullAll
db.usuarios.updateOne(
    {_id: 1},
    { $pull: { hobbies: { $in: [ "pesca", "dança" ] } } }
)
```

---------
