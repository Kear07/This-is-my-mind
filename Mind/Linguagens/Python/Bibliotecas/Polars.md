------------
```Python
import polars as pl 
```

O Polars não possui índices (index) como o Pandas. Ele opera com duas APIs principais:

- **Eager API**: Execução imediata (similar ao Pandas).
- **Lazy API**: Cria um plano de execução otimizado e roda apenas quando solicitado (`.collect()`).

-------
### Leitura (I/O)

``` Python
# Criar do zero
df = pl.DataFrame(
    {"colunaA": [1, 2, 3], "colunaB": ["A", "B", "C"]}
)

# Ler de arquivos (Modo Eager - carrega na memória)
df_csv = pl.read_csv("arquivo.csv")
df_json = pl.read_json("arquivo.json")
# df_excel = pl.read_excel("arquivo.xlsx") # Requer dependências extras (ex: fastexcel)

# Ler de arquivos (Modo Lazy - Recomendado para Big Data)
# Cria apenas o plano, não carrega os dados ainda
lf_csv = pl.scan_csv("arquivo.csv")
```

- `pl.DataFrame(dict)`: Cria um DataFrame.
- `pl.read_csv()`: Lê CSV para memória.
- `pl.scan_csv()`: Lê CSV de forma "preguiçosa" (LazyFrame).
- `pl.read_json()`: Lê arquivo JSON.
- `pl.read_parquet()`: Lê arquivos Parquet (formato nativo e muito rápido).

----
### Conversão (I/O)

```Python
# Polars não tem índice, então não precisa de index=False
df.write_csv("meu_arquivo.csv", separator=";")
df.write_excel("meu_arquivo.xlsx")
df.write_json("meu_arquivo.json")
df.write_parquet("meu_arquivo.parquet")
```

- `df.write_csv(path)`: Salva como CSV.
- `df.write_excel(path)`: Salva como Excel.
- `df.write_json(path)`: Salva como JSON.
- `df.write_parquet(path)`: Salva como Parquet (altamente recomendado).

---------
### Exibição e Inspeção

```Python
print(df.head(3))     # Exibe as 3 primeiras linhas
print(df.shape)       # (Nº de linhas, Nº de colunas)
print(df.glimpse())   # Resumo denso (melhor que .info() do Pandas)
print(df.describe())  # Estatísticas das colunas
print(df.columns)     # Lista os nomes das colunas
print(df.schema)      # Dicionário com nomes e tipos
```

- `df.head(n)`: Exibe as primeiras `n` linhas.
- `df.glimpse()`: Mostra estrutura, tipos e amostra de dados compacta.
- `df.shape`: Retorna tupla (linhas, colunas).
- `df.describe()`: Exibe estatísticas gerais.
- `df.schema`: Mostra os tipos de dados (DataType) de cada coluna.
- `df.estimated_size()`: Estima o uso de memória em bytes.

----------
### Seleção e Filtragem (Contexto de Expressões)

**Diferença Vital:** Use `select` para colunas e `filter` para linhas. Evite colchetes `[]` para lógica complexa.

```Python
# 1. Seleção de Colunas (.select)
df_cols = df.select(pl.col("colunaA"))       # Retorna DataFrame
df_cols = df.select(["colunaA", "colunaB"])  # Múltiplas colunas

# 2. Exclusão de Colunas
df_limpo = df.select(pl.exclude("coluna_inutil"))

# 3. Filtragem de Linhas (.filter)
# Use sempre pl.col() para referenciar a coluna
filtro_idade = df.filter(pl.col("Idade") > 25)
filtro_complexo = df.filter(
    (pl.col("Idade") > 25) & (pl.col("Cidade") == "SP")
)

# 4. Fatiamento (Slice)
fatia = df.slice(0, 5) # Começa no 0, pega 5 linhas
```

- `df.select(exprs)`: Seleciona e transforma colunas.
- `df.filter(expr_bool)`: Filtra linhas baseado em condição.
- `pl.col("nome")`: **A função mais importante.** Cria uma expressão referenciando uma coluna.
- `pl.exclude("nome")`: Seleciona tudo exceto a coluna especificada.

--------
### Manipulação e Transformação

No Polars, você raramente atribui direto (`df['col'] = ...`). Use **`.with_columns()`**.

```Python
# Renomear
df = df.rename({"old_name": "new_name"})

# Remover colunas
df = df.drop("coluna_ruim")

# Criar ou Modificar Colunas
df = df.with_columns([
    # Cast de tipo
    pl.col("Idade").cast(pl.Int64),
    
    # Operação matemática
    (pl.col("Idade") * 2).alias("Idade_Dobro"),
    
    # Condicional (If-Else)
    pl.when(pl.col("Idade") >= 18)
      .then(pl.lit("Maior"))
      .otherwise(pl.lit("Menor"))
      .alias("Status_Civil")
])

# Mapeamento (Map)
df = df.with_columns(
    pl.col("Sexo").replace({"M": 0, "F": 1}, default=None)
)
```

- `df.with_columns([exprs])`: Adiciona ou substitui colunas.
- `expr.alias("nome")`: Renomeia a coluna resultante da expressão.
- `pl.lit(valor)`: Cria um valor literal (constante) para usar em expressões.
- `pl.when(cond).then(val).otherwise(val)`: Estrutura condicional ternária.
- `expr.cast(tipo)`: Converte o tipo de dado.

#### Manipulação de Nulos

```Python
df.select(pl.col("col").is_null()) # Retorna booleanos
df.fill_null(0)                    # Preenche nulos com literal
df.fill_null(strategy="forward")   # Preenche com valor anterior
df.drop_nulls()                    # Remove linhas com nulos
```

------
### Estatísticas e Agregação

```Python
# Estatísticas simples
df.select([
    pl.col("Idade").mean(),
    pl.col("Idade").max(),
    pl.col("Cidade").n_unique()
])

# Value Counts
contagem = df["Cidade"].value_counts()
```

- `expr.sum()`, `.mean()`, `.min()`, `.max()`: Agregações padrão.
- `expr.n_unique()`: Conta valores únicos.
- `expr.value_counts()`: Retorna contagem de frequência.

#### Agregação (Group By)

```Python
# Group By + Agg (Sintaxe preferida)
df_agg = df.group_by("Cidade").agg([
    pl.col("Idade").mean().alias("media_idade"),
    pl.col("Salário").max().alias("max_salario"),
    pl.len().alias("qtd_pessoas") # Contagem de linhas no grupo
])
```

- `df.group_by("col")`: Inicia agrupamento.
- `.agg([exprs])`: Define como agregar as colunas.
- `pl.len()` ou `pl.count()`: Conta o número de elementos no grupo.

----------
### Junção de Dados (Join e Concat)

```Python
# Concatenação Vertical (Empilhar)
df_total = pl.concat([df1, df2], how="vertical")

# Join (Merge)
df_merged = df_pedidos.join(
    df_clientes, 
    on="ID_Cliente", 
    how="left" # 'inner', 'left', 'outer', 'semi', 'anti', 'cross'
)
```

- `pl.concat(lista)`: Junta DataFrames.
- `df.join(other, on, how)`: Realiza a junção.
- `how='anti'`: Retorna linhas do esquerdo que **não** existem no direito (muito útil).

---
### Ordenação

```Python
# Ordenar
df_ordenado = df.sort("Idade", descending=True)

# Múltiplas colunas
df_ordenado = df.sort(["Cidade", "Idade"], descending=[False, True])
```

- `df.sort(by, descending=False)`: Ordena o DataFrame.

-------
### Manipulação de Strings (.str)

```Python
df = df.with_columns([
    pl.col("Nome").str.to_lowercase(),
    pl.col("Nome").str.strip_chars(),          # Igual .strip()
    pl.col("Nome").str.contains("Silva"),      # Retorna Boolean
    pl.col("Nome").str.replace("Dr.", "")      # Substituição
])
```

- `expr.str.to_lowercase()`: Minúsculas.
- `expr.str.strip_chars()`: Remove espaços extras.
- `expr.str.contains(padrao)`: Busca texto (regex ou literal).
- `expr.str.slice(start, length)`: Corta a string.

---------

