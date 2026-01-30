--------
# Joins (Combinação Horizontal)

Joins combinam colunas de dois DataFrames baseados em uma chave comum.
(https://stackoverflow.com/questions/13997365/sql-joins-as-venn-diagram)

----
# Sintaxe Básica
`df1.join(df2, on="chave", how="tipo")`

```python
# Dados de exemplo
df_vendas = spark.createDataFrame([(1, 100), (2, 200), (3, 300)], ["id_venda", "valor"])
df_clientes = spark.createDataFrame([(1, "Ana"), (2, "Beto"), (4, "Dani")], ["id_venda", "nome"])

# Join Inner (Padrão) - Traz apenas o que tem correspondência em ambos
df_vendas.join(df_clientes, on="id_venda", how="inner").show()
```

---
# Tipos de Join (`how`)

| **Tipo**                | **Descrição**                                                                            |
| ----------------------- | ---------------------------------------------------------------------------------------- |
| `inner`                 | Retorna apenas linhas com chaves correspondentes em ambos (Interseção).                  |
| `left` / `left_outer`   | Retorna tudo da esquerda e correspondências da direita (nulos se não houver).            |
| `right` / `right_outer` | Retorna tudo da direita e correspondências da esquerda.                                  |
| `outer` / `full`        | Retorna tudo de ambos os lados (união completa).                                         |
| `cross`                 | Produto cartesiano (cada linha de A combinas com todas de B). Perigoso para performance! |

------
# Joins Especiais (Filtros)

Muito úteis para verificar qualidade de dados ou exclusões.

- **`left_semi`**: Retorna as linhas da esquerda que **TÊM** correspondência na direita (mas não traz colunas da direita). É como um filtro `EXISTS`.    
- **`left_anti`**: Retorna as linhas da esquerda que **NÃO TÊM** correspondência na direita. Útil para achar "registros órfãos".

```python
# Achar vendas sem cliente cadastrado
vendas_orfas = df_vendas.join(df_clientes, on="id_venda", how="left_anti")
```

## O Problema da Ambiguidade

Se os dois DataFrames tiverem colunas com o mesmo nome (ex: ambos tem `id`), o Spark vai criar `id` e `id`. Se você tentar dar um `select("id")` depois, ele vai dar erro de ambiguidade.
**Solução 1: Join com string (automático)** Se o nome da coluna chave for igual, passe apenas a string. O Spark funde as colunas.

```python
df1.join(df2, "id") # Resultado tem apenas uma coluna 'id'
```

Solução 2: Renomear antes

```python
df2 = df2.withColumnRenamed("id", "id_cliente")
df1.join(df2, df1.id == df2.id_cliente)
```

-----
# Unions (Combinação Vertical)

Unions empilham linhas de DataFrames com a mesma estrutura.

## `union()` vs `unionByName()`

- **`union()`**: Empilha por **posição**. A coluna 1 do DF A vai em cima da coluna 1 do DF B, independente do nome. Perigoso se a ordem estiver trocada!    
- **`unionByName()`**: Empilha pelo **nome** da coluna. Mais seguro.

```python
df_jan = spark.createDataFrame([(1, "A")], ["id", "tipo"])
df_fev = spark.createDataFrame([("B", 2)], ["tipo", "id"]) # Ordem trocada

# ERRADO (Mistura ID com Tipo)
df_jan.union(df_fev).show() 

# CERTO (Alinha as colunas corretamente)
df_jan.unionByName(df_fev).show()
```

---------
