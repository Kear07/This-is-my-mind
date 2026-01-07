--------------

`import pandas as pd`

O Pandas possui duas estruturas de dados principais:
* **`Series`**: Um array unidimensional (como uma coluna de tabela).
* **`DataFrame`**: Uma tabela bidimensional (linhas e colunas).
### Leitura (I/O)

```python
# Criar do zero
df = pd.DataFrame(
    {"colunaA": [1, 2, 3], "colunaB": ["A", "B", "C"]}
)

# Ler de arquivos
df_csv = pd.read_csv("arquivo.csv")
df_excel = pd.read_excel("arquivo.xlsx", sheet_name="Aba1")
df_json = pd.read_json("arquivo.json")
# df_html = pd.read_html("[http://site.com/tabela](http://site.com/tabela)") # Retorna lista de DataFrames
```

- `pd.DataFrame(dict)`: Cria um DataFrame.
- `pd.Series(lista)`: Cria uma Series (vetor unidimensional).
- `pd.read_csv()`: Lê um arquivo CSV.
- `pd.read_excel()`: Lê um arquivo Excel.
- `pd.read_json()`: Lê um arquivo JSON.
- `pd.read_html()`: Lê tabelas de uma página HTML.

------------
### Conversão (I/O)

```python
# O 'index=False' é crucial para não salvar o índice do Pandas como uma coluna
df.to_csv("meu_arquivo.csv", index=False, sep=";")
df.to_excel("meu_arquivo.xlsx", index=False)
df.to_json("meu_arquivo.json", orient="records")
```

- `df.to_csv(path, index=False)`: Salva como CSV.
- `df.to_excel(path, index=False)`: Salva como Excel.
- `df.to_json(path)`: Salva como JSON.
- `df.to_html()`: Retorna uma string com a tabela em HTML.

-------
### Exibição e Inspeção

```python
print(df.head(3))     # Exibe as 3 primeiras linhas
print(df.shape)       # (Nº de linhas, Nº de colunas)
print(df.info())      # Resumo (tipos, nulos, memória)
print(df.describe())  # Estatísticas de colunas numéricas
print(df.columns)     # Lista os nomes das colunas
print(df.dtypes)      # Mostra o tipo de dado de cada coluna
```

- `df.head(n)`: Exibe as primeiras `n` linhas (padrão 5).
- `df.tail(n)`: Exibe as últimas `n` linhas (padrão 5).
- `df.shape`: Retorna uma tupla (linhas, colunas).
- `df.info()`: Resumo das colunas, tipos e valores não nulos.
- `df.describe()`: Exibe estatísticas (média, mediana, desvio padrão, etc.).
- `df.columns`: Exibe o nome das colunas.
- `df.index`: Exibe o índice do DataFrame.
- `df.dtypes`: Exibe os tipos de dados de cada coluna.

------
### Seleção e Filtragem de Dados

**Correção Importante:** Para selecionar múltiplas colunas, você deve passar uma **lista** (colchetes duplos).

```python
# 1. Seleção de Colunas
coluna_a = df['colunaA']              # Retorna uma Series
colunas_ab = df[['colunaA', 'colunaB']] # Retorna um DataFrame

# 2. Seleção de Linhas (baseada em Posição Inteira)
primeira_linha = df.iloc[0]          # Retorna a primeira linha
primeiras_duas = df.iloc[0:2]        # Fatiamento (linhas 0 e 1)

# 3. Seleção (baseada em Rótulo/Label) - Poderoso
linha_com_index_5 = df.loc[5]
fatia = df.loc[5:10, ['colunaA', 'colunaB']] # Linhas 5-10, colunas A e B

# 4. Filtragem Booleana (a forma mais comum)
filtro_idade = df[df['Idade'] > 25]
filtro_complexo = df[(df['Idade'] > 25) & (df['Cidade'] == 'SP')]

# 5. Query (alternativa para filtro)
filtro_query = df.query('Idade > 25 and Cidade == "SP"')
```

- `df['coluna']`: Seleciona uma coluna.
- `df[['col1', 'col2']]`: Seleciona múltiplas colunas.
- `df.iloc[pos]` / `df.iloc[linha_pos, col_pos]`: Seleção por **posição** inteira.
- `df.loc[label]` / `df.loc[linha_label, col_label]`: Seleção por **rótulo** (label do índice).
- `df[df['Coluna'] > 25]`: Filtragem booleana.
- `df.query('coluna > 25')`: Seleciona linhas com base em uma expressão string.

------
### Manipulação e Transformação

```python
# Renomear
df = df.rename(columns={'old_name': 'new_name'})

# Remover colunas (axis=1) ou linhas (axis=0)
df_sem_coluna = df.drop('coluna_inutil', axis=1)
df_sem_linha = df.drop(5, axis=0) # Remove linha com índice 5

# Mudar tipo
df['Idade'] = df['Idade'].astype(int)

# Aplicar funções
# .map() - (para Series) Mapeia valores
df['Sexo_Binario'] = df['Sexo'].map({'M': 0, 'F': 1})
# .apply() - Aplica uma função
df['Idade_Dobro'] = df['Idade'].apply(lambda x: x * 2)
# .apply(axis=1) - (para DataFrame) Aplica função em linhas
df['Nome_Idade'] = df.apply(lambda linha: f"{linha['Nome']} - {linha['Idade']}", axis=1)

# Substituir valores
df['Cidade'] = df['Cidade'].replace({'SP': 'São Paulo'})
```

- `df.rename(columns={ 'old': 'new' })`: Renomeia colunas.
- `df.drop('nome', axis=1)`: Remove uma coluna.
- `df.drop(index_label, axis=0)`: Remove uma linha pelo índice.
- `df['col'].astype(tipo)`: Converte o tipo de uma coluna.
- `df.replace({ 'A': 'abc' })`: Substitui valores.
- `df['col'].map(dict_ou_func)`: Mapeia valores de uma Series.
- `df.apply(func, axis=0|1)`: Aplica uma função no DataFrame (por coluna `axis=0` ou linha `axis=1`).
#### Manipulação de Nulos (NaN)

```python
df.isna() # Retorna DataFrame de booleans (True se for Nulo)
df.fillna("Valor_Padrao") # Preenche Nulos com um valor
df.dropna() # Remove linhas que contêm qualquer valor nulo
```

- `df.isna()` / `df.isnull()`: Verifica se há valores nulos.
- `df.notna()` / `df.notnull()`: Retorna True para valores não nulos.
- `df.fillna(valor)`: Preenche valores nulos (NaN) com `valor`.
- `df.dropna()`: Remove linhas/colunas com valores nulos.
#### Duplicatas

- `df.duplicated()`: Retorna booleano para linhas duplicadas.
- `df.drop_duplicates()`: Remove linhas duplicadas.

------
### Estatísticas

```python
df['Idade'].sum()
df['Idade'].mean()
df['Idade'].median()
df['Idade'].std() # Desvio Padrão
df['Idade'].min()
df['Idade'].max()
df['Cidade'].value_counts() # Conta ocorrências de cada Cidade
df['Cidade'].nunique() # Conta valores únicos (ex: 27)
```

- `df['col'].sum()`: Soma.
- `df['col'].mean()`: Média.
- `df['col'].median()`: Mediana.
- `df['col'].min()` / `.max()`: Mínimo / Máximo.
- `df['col'].count()`: Conta elementos não nulos.
- `df['col'].value_counts()`: **(Muito útil)** Conta valores únicos em uma Series.
- `df['col'].nunique()`: Conta o número de valores únicos.

------------
### Agregação (Group By) 

O `groupby` segue o padrão "Split-Apply-Combine" (Dividir-Aplicar-Combinar).

```python 
# 1. Agrupar por 'Cidade' e calcular a média de 'Idade' e 'Salário'
df_agrupado = df.groupby('Cidade')[['Idade', 'Salário']].mean()

# 2. Agrupar por múltiplas colunas
df_agrupado_multi = df.groupby(['Cidade', 'Sexo']).size()

# 3. Agregação Múltipla (.agg) - O mais poderoso
df_agg = df.groupby('Cargo').agg(
    media_salario=('Salário', 'mean'),
    max_idade=('Idade', 'max'),
    contagem_pessoas=('Nome', 'count')
)
```

- `df.groupby('col_grupo')`: Cria um objeto GroupBy.
- `df.groupby('col_grupo')['col_valor'].mean()`: Agrupa e aplica uma função.
- `df.groupby('col_grupo').agg({ 'col_A': 'mean', 'col_B': 'sum' })`: Aplica diferentes agregações em diferentes colunas.

------
### Junção de Dados (Merge e Concat)

```python 
# Concat (Empilhar DataFrames - axis=0)
df_total = pd.concat([df1, df2]) 
# Concat (Juntar lado a lado - axis=1)
df_lado = pd.concat([df1, df2], axis=1)

# Merge (Similar ao JOIN do SQL)
df_merged = pd.merge(
    df_pedidos, 
    df_clientes, 
    on='ID_Cliente', # Coluna chave
    how='left'      # Tipos: 'inner', 'left', 'right', 'outer'
)
```

- `pd.concat([df1, df2], axis=0)`: Empilha linhas (eixo 0).
- `pd.concat([df1, df2], axis=1)`: Junta colunas (eixo 1).
- `pd.merge(df_esq, df_dir, on='chave', how='inner')`: Junta DataFrames baseado em uma coluna chave (como `JOIN` do SQL).

--------------
### Ordenação

```python 
# Ordenar pelas idades (decrescente)
df_ordenado = df.sort_values(by='Idade', ascending=False)
# Resetar o índice (o índice antigo vira uma coluna)
df_ordenado = df_ordenado.reset_index(drop=False)
```

- `df.sort_values(by='coluna', ascending=False)`: Ordena com base em valores.    
- `df.sort_index()`: Ordena com base no índice.
- `df.reset_index(drop=True)`: Reseta o índice para `0, 1, 2...` (útil após filtros).

-----
### Manipulação de Strings (.str)

```python 
df['Nome'] = df['Nome'].str.lower()
df['Nome'] = df['Nome'].str.strip()
df['Nome'] = df['Nome'].str.replace('Dr.', '')
df_filtro = df[df['Nome'].str.contains('Silva')]
```

- `df['col'].str.lower()`: Converte para minúsculas.
- `df['col'].str.upper()`: Converte para maiúsculas.
- `df['col'].str.strip()`: Remove espaços em branco no início e fim.
- `df['col'].str.contains('texto')`: Retorna `True`/`False` (bom para filtros).
- `df['col'].str.replace('A', 'B')`: Substitui texto

--------
