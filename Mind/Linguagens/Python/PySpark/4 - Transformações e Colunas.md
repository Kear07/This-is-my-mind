-----
#  Transformações e Colunas

No PySpark, os DataFrames são **imutáveis**. Isso significa que métodos como `withColumn` ou `filter` não mudam o objeto original; eles retornam um **novo DataFrame** com as alterações aplicadas.

Para manipular colunas, importamos a biblioteca de funções padrão:

```python
from pyspark.sql.functions import col, lit, when, expr
```

-----
# Select (Selecionar Colunas)

Você pode usar strings ou objetos `col()`. O objeto `col` é preferível pois permite operações matemáticas dentro do select.

```python
# Strings
df.select("nome", "idade").show()

# Objetos col (permite operações)
df.select(col("nome"), col("idade") + 1).show()
```

-----
# Filter / Where (Filtrar Linhas)

```python
# Filtro simples
df.filter(col("idade") > 18).show()

# Múltiplas condições (& = AND, | = OR, ~ = NOT)
# Atenção: Parênteses são obrigatórios em múltiplas condições!
df.filter((col("idade") > 18) & (col("pais") == "Brasil")).show()
```

-----
# Criar e Modificar Colunas (`withColumn`)

Esta é a função principal para transformações. Sintaxe: `.withColumn("nome_da_coluna", transformação)`

## Adicionar coluna calculada

```python
# Cria uma coluna "idade_meses" multiplicando a idade por 12
df_novo = df.withColumn("idade_meses", col("idade") * 12)
```

## Adicionar valor constante (`lit`)

Para adicionar um valor fixo (literal) a todas as linhas, usamos `lit`. O Spark falha se você tentar passar apenas o número/string cru.

```python
df_novo = df.withColumn("status_proc", lit("processado"))
```

## Lógica Condicional (`when` / `otherwise`)

Equivalente ao `CASE WHEN` do SQL ou `if/else` do Excel.

```python
df_novo = df.withColumn("faixa_etaria", 
    when(col("idade") < 18, "Menor")
    .when((col("idade") >= 18) & (col("idade") < 60), "Adulto")
    .otherwise("Idoso")
)
```

-----
# Renomear e Remover

## Renomear (`withColumnRenamed`)

```python
# Sintaxe: (nome_antigo, nome_novo)
df_renomeado = df.withColumnRenamed("dt_nasc", "data_nascimento")
```

## Remover (`drop`)

```python
df_limpo = df.drop("coluna_inutil", "outra_coluna")
```

-----
# Dica: Expressões SQL (`expr`)

Às vezes é mais fácil escrever a lógica como uma string SQL do que encadear funções Python.

```python
# Concatenação simples usando expr
df.withColumn("nome_completo", expr("nome || ' ' || sobrenome"))

# É o mesmo que:
# from pyspark.sql.functions import concat_ws
# df.withColumn("nome_completo", concat_ws(" ", col("nome"), col("sobrenome")))
```

--------
