---------------
### CREATE (Criar)

Usado para criar novos nós e relações que **não existem**.

```cypher
// 1. Criar um Nó simples
CREATE (p:Pessoa)

// 2. Criar um Nó com Rótulos e Propriedades
CREATE (k:Pessoa:Programador {nome: "Kear", idade: 25})

// 3. Criar Relações (DEVE ser entre nós existentes)
// Primeiro, encontramos os nós:
MATCH (p:Pessoa {nome: "Kear"})
MATCH (c:Cidade {nome: "São Paulo"})

// Depois, criamos a relação entre eles
CREATE (p) -[r:NASCEU_EM]-> (c)

// 4. Criar Nós E Relações AO MESMO TEMPO
// (O padrão mais comum para adicionar dados novos)
CREATE (ana:Pessoa {nome: "Ana"}) -[:AMIGA_DE]-> (beto:Pessoa {nome: "Beto"})
```

**Importante:** `CREATE` _sempre_ cria duplicatas se você rodar o comando de novo (ex: você terá duas Anas amigas de dois Betos). Para evitar isso, usamos `MERGE`.

------------
### MERGE (O "Upsert")

O `MERGE` é o comando mais importante para a escrita. Ele significa: "Encontre este padrão; se ele existir, use-o. Se não, crie-o."

É uma combinação de `MATCH` + `CREATE`.

```cypher
// 1. MERGE em um Nó (O "Get or Create")
// Tenta encontrar uma Pessoa chamada "Kear".
// Se não encontrar, CRIA uma Pessoa chamada "Kear".
MERGE (p:Pessoa {nome: "Kear"})
RETURN p

// 2. MERGE com ON CREATE e ON MATCH
// (Permite definir propriedades diferentes para criação vs. atualização)
MERGE (p:Pessoa {nome: "Kear"})
    ON CREATE SET p.criadoEm = timestamp(), p.primeiroLogin = true
    ON MATCH SET p.ultimoLogin = timestamp()
RETURN p
```

#### Criando Relações com `MERGE`

Este é o padrão-ouro para conectar dois nós que você sabe que já existem.

```cypher
// 1. Encontra o usuário
MATCH (u:Usuario {id: 123})
// 2. Encontra o produto
MATCH (p:Produto {id: 456})

// 3. MERGE (cria) a relação entre eles.
//    Isso garante que a relação :COMPROU só seja criada UMA VEZ.
MERGE (u) -[r:COMPROU {data: "2025-11-05"}]-> (p)
```

-----------
### SET (Atualizar Propriedades)

Usado para adicionar, atualizar ou remover propriedades de Nós ou Relações existentes.

```cypher
// 1. Encontra o nó
MATCH (p:Pessoa {nome: "Kear"})

// 2. Atualiza (ou adiciona) uma propriedade
SET p.idade = 26
SET p.email = "kear@email.com"

// 3. Substitui TODAS as propriedades (raro, mas útil)
SET p = {nome: "Kear_07", idade: 26, status: "ativo"}

// 4. Adiciona um novo Rótulo (Label)
SET p:Dev:Java
```

---------
### REMOVE (Remover Propriedades ou Rótulos)

```cypher
MATCH (p:Pessoa {nome: "Kear_07"})

// Remove uma propriedade específica
REMOVE p.idade

// Remove um Rótulo
REMOVE p:Java
```

-------
### DELETE (Remover Nós e Relações)

Você **não pode** deletar um Nó que ainda tenha relações ligadas a ele.

```cypher
// 1. Encontra um nó "folha" (sem relações) e o deleta
MATCH (p:Pessoa {nome: "Usuario_Solitario"})
DELETE p

// 2. ERRO: Tenta deletar um nó que tem relações
MATCH (p:Pessoa {nome: "Kear"})
DELETE p
// ERRO: "Cannot delete node<X>, because it still has relationships."

// 3. A Solução: DETACH DELETE
// "Destaque" (DETACH) todas as relações do nó e DEPOIS o delete.
MATCH (p:Pessoa {nome: "Kear"})
DETACH DELETE p
```

-------
