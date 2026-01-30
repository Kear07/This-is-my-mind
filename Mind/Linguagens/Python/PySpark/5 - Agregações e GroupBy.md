-----
#  Agregações e GroupBy

Agregações são usadas para transformar múltiplas linhas em um único resultado (ex: soma de vendas, contagem de usuários).

Para usar as funções de agregação, é necessário importá-las:
```python
from pyspark.sql.functions import count, sum, avg, min, max, countDistinct
```

------
# Agregação Simples (Sem Grupo)

Quando você quer uma estatística do DataFrame inteiro, não precisa usar o `groupBy`.

```python
# Retorna um DataFrame com uma linha
df.select(
    count("*").alias("total_linhas"),
    sum("vendas").alias("total_vendas"),
    avg("idade").alias("media_idade")
).show()
```

-----
# GroupBy Básico

O `groupBy` agrupa os dados baseados em valores idênticos de uma ou mais colunas.

```python
# Contagem simples por categoria
df.groupBy("categoria").count().show()
```

Nota: O método `.count()` é o único que pode ser chamado diretamente após o `groupBy` sem precisar de importação extra.

---
# A Função `.agg()` (A maneira correta)

Se você quiser calcular **mais de uma métrica** ao mesmo tempo (ex: soma E média), ou se quiser renomear as colunas de saída, deve usar o método `.agg()`.

```python
from pyspark.sql.functions import sum, round

df_resumo = df.groupBy("departamento").agg(
    sum("salario").alias("folha_salarial"),
    round(avg("salario"), 2).alias("salario_medio"),
    max("salario").alias("maior_salario")
)

df_resumo.show()
```

----
# Count vs CountDistinct

Em Big Data, saber a diferença é crucial.

- `count()`: Conta ocorrências totais (incluindo duplicados).
- `countDistinct()`: Conta valores únicos (ex: quantos clientes _diferentes_ compraram).

```python
df.groupBy("data").agg(
    count("id_transacao").alias("volume_transacoes"),
    countDistinct("id_cliente").alias("clientes_unicos")
).show()
```

## Dica de Performance: `approx_count_distinct`

Se você tem bilhões de linhas, calcular o `countDistinct` exato é muito lento (exige _shuffle_ pesado). Se uma margem de erro pequena for aceitável (ex: dashboards de tendência), use:

```python
from pyspark.sql.functions import approx_count_distinct

# Muito mais rápido para grandes volumes
df.agg(approx_count_distinct("id_usuario")).show()
```

----
# Pivot (Transposição)

O Spark permite transformar valores de linhas em colunas (pivotar).

```python
# Transforma os meses (jan, fev...) em colunas
df_pivot = df.groupBy("produto") \
    .pivot("mes") \
    .sum("vendas")
```

---
