--------
#  Lendo Dados (DataFrameReader)

A estrutura básica é: `spark.read.format(...).options(...).load(...)`.

## Lendo CSV
O CSV é comum, mas não carrega metadados de tipo, exigindo `inferSchema` (lento) ou schema manual.
```python
df_csv = spark.read \
    .format("csv") \
    .option("header", "true") \
    .option("delimiter", ";") \
    .option("inferSchema", "true") \
    .load("caminho/arquivo.csv")
```

## Lendo JSON

Útil para dados semi-estruturados.

```python
# multiline=true é necessário se o JSON ocupar várias linhas por objeto
df_json = spark.read \
    .option("multiline", "true") \
    .json("caminho/dados.json")
```

## Lendo Parquet (O Padrão Ouro)

O Parquet é um formato **colunar**, comprimido e que preserva o schema. É o formato nativo do Spark.

```python
# Não precisa de options de header ou schema, pois o metadado já está no arquivo
df_parquet = spark.read.parquet("caminho/dados.parquet")
```

------
## Row-based vs Column-based

Entender porque preferimos Parquet (Colunar) ao CSV (Linha) é vital para performance em Big Data.

- **CSV (Row):** Para ler a coluna "Idade", o computador tem que ler a linha inteira (Nome, Endereço, etc.) e descartar o resto. Lento para _Analytics_.
- **Parquet (Column):** Os dados da coluna "Idade" estão guardados juntos no disco. O Spark lê apenas o que precisa.

------
# Escrevendo Dados (DataFrameWriter)

A estrutura básica é: `df.write.mode(...).format(...).save(...)`.

## Save Modes (Modos de Escrita)

O `.mode()` define o comportamento se o arquivo de destino já existir:

| **Modo**          | **Comportamento**                                                         |
| ----------------- | ------------------------------------------------------------------------- |
| `append`          | Adiciona os novos dados ao final dos existentes (cuidado com duplicação). |
| `overwrite`       | Apaga tudo o que existe no destino e escreve os novos dados.              |
| `error` (default) | Lança um erro se o destino já existir.                                    |
| `ignore`          | Se existir, não faz nada (não escreve e não dá erro).                     |
## Exemplo de Escrita

```python
# Convertendo um CSV processado para Parquet (Melhor prática)
df_csv.write \
    .mode("overwrite") \
    .parquet("caminho/saida_otimizada")
```

## PartitionBy (Particionamento)

Ao salvar, você pode organizar os dados em pastas baseadas em uma coluna. Isso acelera filtros futuros.

```python
# Cria pastas tipo: /ano=2024/mes=01/dados.parquet
df.write \
    .partitionBy("ano", "mes") \
    .parquet("caminho/vendas")
```

-----
