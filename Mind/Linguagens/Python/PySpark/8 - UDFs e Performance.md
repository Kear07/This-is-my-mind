------
#  UDFs (User Defined Functions)

As UDFs permitem aplicar uma função Python arbitrária a cada linha de um DataFrame.

**⚠️ O Problema de Performance:**
O Spark roda na JVM (Java), mas a sua função é Python. Para cada linha, o Spark precisa:
1.  Serializar os dados (Java -> Python).
2.  Executar a função Python num processo separado.
3.  Deserializar o resultado (Python -> Java).

Isso "quebra" o otimizador do Spark (Catalyst) e é muito lento. **Sempre prefira funções nativas (`pyspark.sql.functions`) se possível.**

### UDF Clássica (Lenta)
```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

# 1. Definir a função Python pura
def caixa_alta_custom(texto):
    if texto:
        return texto.upper() + "!"
    return None

# 2. Registrar como UDF (Obrigatório definir o tipo de retorno!)
udf_upper = udf(caixa_alta_custom, StringType())

# 3. Usar
df.withColumn("gritando", udf_upper("coluna_texto")).show()
```

-----
# Pandas UDF (Vectorized - Rápida)

Introduzidas nas versões mais recentes, usam o **Apache Arrow** para transferir blocos de dados em vez de linha a linha. São muito mais eficientes.

```python
from pyspark.sql.functions import pandas_udf
import pandas as pd

# O decorador define o tipo de retorno
@pandas_udf("double")
def dobrar_valores(series: pd.Series) -> pd.Series:
    return series * 2

df.withColumn("dobro", dobrar_valores("coluna_numerica")).show()
```

----
# Lazy Evaluation & Catalyst Optimizer

O Spark é **preguiçoso** (Lazy).

- **Transformações (Lazy):** `filter`, `select`, `join`. O Spark apenas anota o plano, não executa nada.
    
- **Ações (Eager):** `show`, `count`, `write`. Aqui o Spark realmente processa os dados.
    

Isso permite que o **Catalyst Optimizer** otimize o plano de execução (ex: filtrando dados antes de fazer um join, conhecido como _Predicate Pushdown_).

Para ver o plano que o Spark criou:

```python
df.explain(True)
```

-----
# Caching e Persistência

Por padrão, o Spark recalcula todo o DAG (Grafo de execução) a cada ação. Se você vai usar o mesmo DataFrame várias vezes (ex: treinar um modelo de ML, depois salvar num banco, depois contar erros), deve colocá-lo em cache.

## `.cache()` vs `.persist()`

- `df.cache()`: Atalho para persistir apenas em **Memória** (MEMORY_AND_DISK). Se acabar a RAM, vai pro disco.
    
- `df.persist(StorageLevel)`: Permite controle total (apenas disco, memória serializada, replicar em 2 nós, etc.).

```python
from pyspark import StorageLevel

# Coloca em cache
df_limpo = df.filter("qualidade = 'boa'").cache()

# Primeira ação (Demora: calcula e salva no cache)
df_limpo.count()

# Segunda ação (Instantânea: lê da RAM)
df_limpo.show()

# IMPORTANTE: Limpar a memória no fim
df_limpo.unpersist()
```

## Quando NÃO usar cache?

- Se o DataFrame é muito grande para a memória e vai "transbordar" (spill) para o disco, o custo de escrever/ler do disco pode ser maior que recalcular.
    
- Se você vai usar o DataFrame apenas uma vez.

-----
