-----------------------
### Simple view 

Uma simple view é basicamente uma consulta SQL salva, literalmente um select pronto, e que se mantem atualizado com o banco.

create view [name] as [select...]               = Cria uma view
create or replece view [name] as [select...]    = Cria ou altera uma view
drop view [name]                                = Exclui uma view

---
### Materialized view  

Praticamente uma simple view, porém seus dados não se atualizam, ou seja, a ultima atualização será no momento de criação. Ela praticamente se torna independente do resto do banco.

create materialized view [name] as [select...]              = Cria uma view materializada
create or replece materialized view [name] as [select...]   = Cria ou altera uma view materializada
drop materialized view [name]                               = Exclui uma view materializada
Refresh materialized view [name]                            = Atualiza a view com os dados atuais

----
### Explain e Explain Analyze

Estas ferramentas mostram como o banco de dados planeja executar sua consulta. Elas revelam se um índice está sendo usado, se está ocorrendo um "Full Table Scan" (escaneamento completo da tabela), e outras informações cruciais para otimização.

EXPLAIN select * from table                                 = Mostra o plano estimado.
EXPLAIN ANALYZE select * from table                         = Executa a consulta e mostra o plano real com tempos.

Termos Chave:
[Sequential Scan / Full Table Scan:] O banco de dados lê todas as linhas da tabela. Geralmente ineficiente para grandes tabelas se houver um índice aplicável.
[Index Scan / Index Only Scan:] O banco de dados usa um índice para encontrar as linhas desejadas. Muito mais rápido quando aplicável.
[Bitmap Heap Scan:] Uma combinação de índice e leitura sequencial, eficiente em alguns cenários.
[Cost:] Estimativa da eficiência da operação (unidades abstratas).
[Rows:] Número estimado (ou real, com ANALYZE) de linhas processadas.
[Time / Actual Time:] Tempo gasto em cada operação (com ANALYZE).

------
### Índices (Indexes)

Um índice em um banco de dados é uma estrutura de dados que melhora a velocidade das operações de busca em uma tabela. 
Pense nele como no final de um livro: em vez de ler o livro inteiro para encontrar um tópico, você consulta o índice, que te diz exatamente em quais páginas o tópico aparece.

Índices podem tornar as operações de INSERT, UPDATE e DELETE mais lentas, pois o índice também precisa ser atualizado.
Use índices estrategicamente onde eles trarão o maior benefício.

Você não notará diferença no select, apenas se for um banco muito populado.

#### Sintaxe criação

CREATE INDEX [idx_vendas_categoria] ON Vendas (categoria)                = Crie um índice na coluna 'categoria' da tabela 'Vendas'.
CREATE INDEX [idx_vendas_data_venda] ON Vendas (data_venda)              = Crie um índice na coluna 'data_venda'.
CREATE INDEX [idx_vendas_categoria_valor] ON Vendas (categoria, valor)   = Crie um índice composto em 'categoria' e 'valor'.
#### Sintaxe update

REINDEX INDEX [idx_vendas_categoria]                                     = Atualiza um index.
REINDEX TABLE [Vendas]                                                   = Reconstrói todos os índices da tabela.
#### Sintaxe de delete

DROP INDEX [idx_vendas_categoria]                                        = Exclui um indice.

-----
