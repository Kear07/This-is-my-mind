------
As **Window Functions** operam num grupo de linhas (uma "janela") relacionadas com a linha atual. Ao contrário do GroupBy, elas **não reduzem** o número de linhas do resultado.

Para utilizá-las, precisamos de importar o objeto `Window`.

```python
from pyspark.sql.window import Window
from pyspark.sql.functions import col, row_number, rank, dense_rank, lag, lead, sum
```

----
# A Anatomia da Janela

Uma especificação de janela (`window spec`) define **como** os dados são fatiados. Ela tem geralmente duas partes:

1. **PartitionBy:** Define os grupos (como se fossem pequenos DataFrames separados).    
2. **OrderBy:** Define a ordem das linhas dentro desse grupo (crucial para rankings).

```python
# Exemplo: Quero analisar salários DENTRO de cada departamento, ordenados do maior para o menor
windowSpec = Window.partitionBy("departamento").orderBy(col("salario").desc())
```

----------
# Ranking (Classificação)

Existem três formas principais de criar rankings. Suponha que temos dois funcionários empatados em 2º lugar.

- **`row_number()`**: Sequencial estrito (1, 2, 3, 4). O desempate é aleatório se não for especificado.    
- **`rank()`**: Pula posições no empate (1, 2, 2, 4).
- **`dense_rank()`**: Não pula posições (1, 2, 2, 3).

```python
df.withColumn("rank", rank().over(windowSpec)) \
  .withColumn("dense_rank", dense_rank().over(windowSpec)) \
  .withColumn("row_num", row_number().over(windowSpec)) \
  .show()
```

-----
# Lag e Lead (Olhar para trás e para frente)

Muito usado para calcular variações (delta) ou crescimento dia-a-dia.

- **`lag(coluna, n)`**: Pega o valor da linha **anterior** (n posições atrás).
- **`lead(coluna, n)`**: Pega o valor da linha **seguinte**.

```python
# Ordenar por data para garantir a sequência temporal
w_tempo = Window.partitionBy("produto").orderBy("data")

# Calcular a diferença de vendas em relação ao dia anterior
df_analise = df.withColumn("vendas_ontem", lag("vendas", 1).over(w_tempo)) \
               .withColumn("diff", col("vendas") - col("vendas_ontem"))

df_analise.show()
```

-----
# Agregações Cumulativas (Running Total)

Para fazer soma acumulada (ex: saldo bancário que muda linha a linha), usamos funções de agregação normais (`sum`, `avg`) sobre uma janela.

Aqui, o `rowsBetween` define o alcance da janela.

- `Window.unboundedPreceding`: Do início da partição...   
- `Window.currentRow`: ...até a linha atual.

```python
w_acumulada = Window.partitionBy("cliente") \
                    .orderBy("data") \
                    .rowsBetween(Window.unboundedPreceding, Window.currentRow)

df = df.withColumn("gasto_acumulado", sum("valor_compra").over(w_acumulada))
```

----
# Resumo Visual

|**Função**|**100**|**90**|**90**|**80**|
|---|---|---|---|---|
|`row_number`|1|2|3|4|
|`rank`|1|2|2|4|
|`dense_rank`|1|2|2|3|

------

\