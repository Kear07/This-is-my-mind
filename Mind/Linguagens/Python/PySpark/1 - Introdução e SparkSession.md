---------
#  Introdução e SparkSession

## O que é PySpark?
O **PySpark** é a interface em Python para o [[Apache Spark]]. Ele permite escrever aplicações de processamento de dados distribuídos usando a sintaxe simples do Python.
- **Spark:** O motor de processamento (escrito em Scala).
- **Py4J:** A biblioteca que permite ao Python comunicar-se com a JVM (Java Virtual Machine) onde o Spark roda.

## Arquitetura Básica
Antes de codar, lembre-se que o Spark funciona em **Cluster** (mesmo que seja local na sua máquina):
1.  **Driver:** O "chefe". Onde seu código Python roda e onde a `SparkSession` é criada.
2.  **Executors:** Os "operários". Processos que executam as tarefas nos dados.
3.  **Cluster Manager:** Quem gerencia os recursos (em modo local, é a própria máquina).

---
# O Ponto de Entrada: SparkSession
Desde o Spark 2.0, a `SparkSession` substituiu o antigo `SparkContext` como ponto de entrada principal. Ela engloba a configuração e a conexão com o cluster.

## Código Base (Boilerplate)

```python
from pyspark.sql import SparkSession

# 1. Construindo a Sessão
# 'local[*]' significa usar todos os núcleos da CPU localmente
spark = SparkSession.builder \
    .master("local[*]") \
    .appName("IniciandoPySpark") \
    .config("spark.ui.port", "4050") \
    .getOrCreate()

# 2. Verificar se está rodando
print(f"Versão do Spark: {spark.version}")
print(f"Aplicação: {spark.sparkContext.appName}")

# O Spark UI fica disponível em http://localhost:4050 enquanto a sessão estiver ativa
```

-----
# Boas Práticas

- Sempre use o método `.getOrCreate()`: Isso evita erros ao tentar instanciar múltiplas sessões onde não é permitido.
- Em ambientes de nuvem (Databricks, AWS EMR), a variável `spark` geralmente já vem inicializada e você não precisa rodar esse bloco.

-----
