--------
### DDL (Data Definition Language) - Estrutura

Comandos usados para definir e modificar a estrutura do banco de dados (tabelas, colunas, tipos).
#### CREATE (Criar)

```sql
/* Esta seção estava faltando */
CREATE TABLE [nome_tabela] (
    id int PRIMARY KEY,
    nome varchar(100) NOT NULL,
    email varchar(100) UNIQUE,
    data_cadastro date DEFAULT CURRENT_DATE
);
```

-----
#### ALTER (Alterar)

```sql
/* Renomear Tabela */
alter table [table] rename to [table2]

/* Colunas */
alter table [table] add [nome tipo]        -- Adiciona uma coluna
alter table [table] drop [colun]         -- Remove uma coluna

/* Restrições (Constraints) */
alter table [table] alter column [colun] set not null -- Não aceita nulos
alter table [table] add unique ([colun])              -- Não aceita repetidos (Correção de 'set unique')

alter table [table] add check (id > 18)                 -- Condição (ex: números)
alter table [table] add check (coluna like 'A%')        -- Condição (ex: texto)

alter table [table] add constraint [nome_restricao] [tipo_restricao] -- Adiciona restrição nomeada
```

---------
#### DROP (Deletar Tabela)

```sql
drop table [table] -- Deleta a tabela (estrutura e dados)
```

--------
#### TRUNCATE (Limpar Tabela)

```sql
truncate table [table]                       -- Deleta os dados da tabela (mais rápido que DELETE)
truncate table [table] restart identity      -- Deleta os dados e reinicia o índice (serial)
```

---
### DML (Data Manipulation Language)

Comandos usados para manipular os dados dentro das tabelas.
#### INSERT (Inserir)

```sql
INSERT INTO [table] (coluna1, coluna2) VALUES (valor1, valor2);
```

--------
#### UPDATE (Atualizar)

```sql
/* Cuidado: 'UPDATE' sem 'WHERE' altera TODAS as linhas. */
update [table] set [colun] = 10 where [id] = 1 -- Altera o valor de uma coluna
```

---------
#### DELETE (Deletar)

```sql
delete from [table] where [colun] < 10 -- Remove linhas baseadas em uma condição
```

------
### TCL (Transaction Control Language) - Transações

Comandos que gerenciam as mudanças feitas pelo DML.

```sql
BEGIN;      -- Inicia uma transação
COMMIT;     -- Salva permanentemente as mudanças (INSERTs, UPDATEs, DELETEs)
ROLLBACK;   -- Desfaz as mudanças desde o último COMMIT
```

------
### Tipos Primitivos (Data Types)

#### Texto

- `varchar` ou `varchar(x)`: String (tamanho variável)
- `char(x)`: String de tamanho fixo [X] (completa com espaços)
- `text`: String de tamanho ilimitado

---------
#### Números Inteiros

- `smallint`: De -32.768 a 32.767
- `int`: De -2.147.483.648 a 2.147.483.648
- `bigint`: Faixa extremamente grande

-----------
#### Números Decimais

- `decimal(prec, esc)` ou `numeric(prec, esc)`: [prec] dígitos totais, [esc] dígitos depois da vírgula (Ex: `decimal(5,2)` -> 999.99)  
- `real`: Precisão flutuante (6 casas decimais)
- `double precision`: Precisão flutuante (15 casas decimais)

----------
#### Autoincremento

- `smallserial`: Contador (smallint)
- `serial`: Contador (int)
- `bigserial`: Contador (bigint)

-------
#### Data & Hora

- `date`: Armazena apenas a data (Ex: '2025-10-21')  
- `time`: Armazena apenas a hora (Ex: '16:45:00')
- `timestamp`: Armazena data E hora
- `timestamptz`: Armazena data E hora COM fuso horário (timezone)

----
