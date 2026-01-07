---------

O `MATCH` é o comando principal para **encontrar** padrões de nós e relações no grafo. Ele é o `SELECT` + `FROM` + `JOIN` do SQL, tudo em um só comando visual.

### Sintaxe Básica: `MATCH` e `RETURN`

A estrutura de uma consulta de leitura é simples:
1.  **`MATCH`**: Descreva o **padrão** que você está procurando.
2.  **`RETURN`**: Especifique o que você quer de volta (nós, relações, propriedades).

```cypher
// Encontra TODOS os nós que têm o rótulo :Pessoa
MATCH (p:Pessoa)
RETURN p

// Encontra um nó específico pela sua propriedade
MATCH (p:Pessoa {nome: "Kear"})
RETURN p

// Encontra e retorna apenas uma propriedade específica
MATCH (p:Pessoa {nome: "Kear"})
RETURN p.idade

// Retorna tudo no banco (CUIDADO: Lento em bancos grandes)
MATCH (n)
RETURN n
```

-------
### Consultando Relações

Aqui é onde o Cypher brilha. Você "desenha" a relação que quer encontrar.

```cypher
// Encontra todos os filmes em que o ator "Tom Hanks" atuou
MATCH (ator:Pessoa {nome: "Tom Hanks"}) -[:ATUOU_EM]-> (filme:Filme)
RETURN filme.titulo

// A direção da seta IMPORTA
// Encontra quem comprou um produto específico
MATCH (cliente:Pessoa) -[:COMPROU]-> (produto:Produto {nome: "Notebook"})
RETURN cliente.nome

// Você pode ignorar a direção se não souber ou não se importar
// (Isso é mais lento, use com moderação)
MATCH (p1:Pessoa) -[:AMIGO_DE]- (p2:Pessoa)
RETURN p1.nome, p2.nome
```

-----------
### Filtrando com `WHERE`

O `MATCH` define o _padrão_ e o `WHERE` define os _filtros_ nas propriedades desse padrão.

```cypher
// Encontra todos os funcionários que ganham mais de 5000
MATCH (f:Funcionario)
WHERE f.salario > 5000
RETURN f.nome, f.salario

// Combinando filtros
// Encontra produtos com preço entre 100 e 500
MATCH (p:Produto)
WHERE p.preco >= 100 AND p.preco <= 500
RETURN p

// Filtrando por propriedades da RELAÇÃO
// Encontra todas as compras feitas em 2025
MATCH (c:Cliente) -[r:COMPROU]-> (p:Produto)
WHERE r.data >= "2025-01-01" AND r.data < "2026-01-01"
RETURN c.nome, p.nome, r.data
```

---------
### `OPTIONAL MATCH` (O "LEFT JOIN")

E se você quiser encontrar um padrão que _pode ou não_ existir? Se você usar `MATCH` e a relação não for encontrada, a consulta inteira retorna _nada_.

O `OPTIONAL MATCH` funciona como um `LEFT JOIN`: ele tenta encontrar o padrão. Se encontrar, ele o retorna; se não, ele retorna `null` para aquela parte, mas **mantém** o resto do resultado.

```cypher
// Queremos a lista de TODOS os usuários, e, SE ELES TIVEREM,
// os pedidos que eles fizeram.

// 1. Encontra todos os usuários (Obrigatório)
MATCH (u:Usuario)

// 2. Tenta encontrar os pedidos (Opcional)
OPTIONAL MATCH (u) -[:FEZ_PEDIDO]-> (p:Pedido)

// 3. Retorna o usuário e seu pedido (que pode ser 'null')
RETURN u.nome, p.id
```

--------

