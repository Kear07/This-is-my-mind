--------------------------------
### Índices (Indexes)

**Por que usar?** Por padrão, uma consulta como `MATCH (p:Pessoa {nome: "Kear"})` força o Neo4j a fazer um "scan" completo, olhando *todas* as `Pessoas` para achar o "Kear". Isso é muito lento.

Um **Índice** é uma estrutura de dados extra que funciona como o índice de um livro: ele mapeia um valor (ex: "Kear") diretamente para o Nó (a "página"), permitindo buscas quase instantâneas.

### Sintaxe `CREATE INDEX`
Você cria índices em um par `(Rótulo, Propriedade)`.

```cypher
// Cria um índice padrão (range index) na propriedade 'nome' 
// para todos os nós com o rótulo 'Pessoa'
CREATE INDEX ON :Pessoa(nome)

// Cria um índice composto em duas propriedades
// (Útil para buscas que filtram por ambos)
CREATE INDEX ON :Produto(categoria, preco)
```

-------------
### Restrições (Constraints)

Constraints são **regras** que protegem a integridade dos seus dados no nível do banco.

**Importante:** Ao criar uma constraint `UNIQUE` ou `KEY`, o Neo4j _automaticamente cria um Índice_ para você. Você não precisa criar os dois.

#### `UNIQUE` (Propriedade Única)

Garante que nenhum outro nó (com o mesmo rótulo) possa ter o mesmo valor para aquela propriedade. Perfeito para emails, CPFs ou IDs de usuário.

```cypher
// Garante que o email seja único entre todas as Pessoas
CREATE CONSTRAINT ON (p:Pessoa) ASSERT p.email IS UNIQUE
```

#### `EXISTS` (Propriedade Obrigatória / "Not Null")

Garante que um nó (ou relação) com aquele rótulo _deve_ ter aquela propriedade.

```cypher
// Garante que todo Produto TENHA um preço
CREATE CONSTRAINT ON (p:Produto) ASSERT p.preco IS NOT NULL

// Garante que toda relação :COMPROU TENHA uma data
CREATE CONSTRAINT ON ()-[r:COMPROU]-() ASSERT r.data IS NOT NULL
```

#### `KEY` (Chave de Nó)

É uma combinação poderosa de `UNIQUE` e `EXISTS` em uma ou mais propriedades. É a forma mais forte de definir uma chave primária em um grafo.

```Cypher
// Garante que todo nó :Usuario DEVE ter um 'id',
// e que esse 'id' DEVE ser único.
CREATE CONSTRAINT ON (u:Usuario) ASSERT u.id IS NODE KEY
```

------------
### Gerenciamento de Estrutura

Como os índices e constraints não são "visíveis" no grafo, você usa comandos `SHOW` e `DROP` para gerenciá-los.

```cypher
// Mostra todos os índices
SHOW INDEXES

// Mostra todas as constraints
SHOW CONSTRAINTS

// --- Para remover ---

// Remove um índice pelo nome (veja o nome com SHOW INDEXES)
DROP INDEX nome_do_indice

// Remove uma constraint pelo nome
DROP CONSTRAINT nome_da_constraint
```

----------
