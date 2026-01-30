-----
# O Conceito de DataFrame
Um **DataFrame** no PySpark é uma coleção distribuída de dados organizada em colunas nomeadas. É conceptualmente equivalente a uma tabela numa base de dados relacional ou a um DataFrame do `pandas`, mas com otimizações para execução distribuída.

----
# Schemas (Esquemas)
O Schema define o nome das colunas e o tipo de dados (String, Integer, Date, etc.). Existem duas formas de definir um schema:

1.  **InferSchema (Implícito):** O Spark lê uma amostra dos dados e tenta adivinhar os tipos. É mais lento e propenso a erros.
2.  **Programmatic Schema (Explícito):** Nós definimos a estrutura manualmente usando `StructType`. É **altamente recomendado** para produção.

---
# Criando um DataFrame com Schema Explícito

Para definir schemas, precisamos importar os tipos do módulo `pyspark.sql.types`.

```python
from pyspark.sql import SparkSession
from pyspark.sql.types import StructType, StructField, StringType, IntegerType, DoubleType

spark = SparkSession.builder.master("local").appName("Schemas").getOrCreate()

# 1. Dados brutos (lista de tuplas)
data = [
    ("Alice", 28, 3500.50),
    ("Bob", 35, 4200.00),
    ("Carlos", None, 2900.00) # Exemplo com valor nulo
]

# 2. Definição do Schema (A Boa Prática)
schema = StructType([
    StructField("nome", StringType(), False),      # False = não aceita nulos
    StructField("idade", IntegerType(), True),     # True = aceita nulos
    StructField("salario", DoubleType(), True)
])

# 3. Criar o DataFrame
df = spark.createDataFrame(data=data, schema=schema)

# 4. Inspecionar
df.printSchema() # Mostra a estrutura em árvore
df.show()        # Mostra os dados (top 20 linhas)
```

## Tipos Comuns (pyspark.sql.types)

- `StringType()`
- `IntegerType()`
- `DoubleType()` / `FloatType()`
- `BooleanType()`
- `DateType()` / `TimestampType()`
- `ArrayType()` (Para listas)
- `MapType()` (Para dicionários/chave-valor)

-----
# Diferença entre `printSchema()` e `dtypes`

- `df.printSchema()`: Imprime a estrutura formatada na consola (visual).
- `df.dtypes`: Retorna uma lista de tuplas `(nome_coluna, tipo)` útil para validações programáticas.

```python
# Exemplo de saída do dtypes
# [('nome', 'string'), ('idade', 'int'), ('salario', 'double')]
print(df.dtypes)
```

-----
