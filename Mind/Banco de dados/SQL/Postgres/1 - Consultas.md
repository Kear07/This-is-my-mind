--------------------
### Estrutura básica do SELECT

SELECT [colunas] FROM [tabela]

---
### DISTINCT
Remove linhas duplicadas do resultado final.

SELECT DISTINCT [coluna] FROM [tabela]

---
### WHERE (Filtros de Linha)

Filtra as linhas ANTES de qualquer agrupamento.

select [*] from table [where id = 1]           

#### Operadores e Argumentos
| Operador | Descrição |
| :--- | :--- |
| `=` | Igual |
| `!=` ou `<>` | Diferente |
| `>` | Maior que |
| `<` | Menor que |
| `>=` | Maior ou igual |
| `<=` | Menor ou igual |
| `AND` | E (ambas as condições devem ser verdadeiras) |
| `OR` | OU (pelo menos uma condição deve ser verdadeira) |
| `IN` | "Em" (compara com uma lista de valores). Ex: `id IN (1, 2, 3)` |
| `NOT` | Negação. Ex: `NOT IN (1, 2, 3)` |
| `BETWEEN [x] AND [y]` | "Entre x e y" (inclusivo) |
| `IS NULL` | É nulo |
| `IS NOT NULL` | Não é nulo |

----
### JOIN (Relacionamentos)

Combina linhas de duas ou mais tabelas com base em uma coluna relacionada.

SELECT [t1.nome] FROM [table1 t1]
JOIN [table2 t2] ON [t1.id] = [t2.table1_id]

#### Tipos de JOIN
| Tipo | Descrição |
| :--- | :--- |
| `JOIN` (ou `INNER JOIN`) | Somente a interseção (linhas que existem em AMBAS as tabelas) |
| `LEFT JOIN` | Todos da tabela da ESQUERDA, mais a interseção |
| `RIGHT JOIN` | Todos da tabela da DIREITA, mais a interseção |
| `FULL JOIN` | Todos de AMBAS as tabelas (interseção + quem só está na esquerda + quem só está na direita) |
| `SELF JOIN` | Um JOIN da tabela com ela mesma (usando aliases diferentes) |

--------------
### GROUP BY (Agrupamento)

Agrupa linhas que têm os mesmos valores em colunas especificadas. Quase sempre usado com Funções de Agregação (COUNT, SUM, AVG, etc.).

SELECT [cidade], [COUNT(id_cliente)] AS [total_clientes]
FROM [clientes]
GROUP BY [cidade];

---
### FILTER (Filtro de Agregação)

Uma forma avançada de filtrar resultados de funções de agregação DENTRO do `SELECT`, sem precisar de sub-queries.

-- Conta clientes ativos e inativos na mesma consulta
SELECT
  COUNT(id) [FILTER] ([WHERE] status = 'ativo') AS "ativos",
  COUNT(id) [FILTER] ([WHERE] status = 'inativo') AS "inativos"
FROM table;

---
### ORDER BY (Ordenação)

Classifica o resultado final da consulta.

SELECT [coluna1], [coluna2] FROM [tabela]
ORDER BY [coluna_para_ordenar ASC | DESC]

`ASC` (Ascendente): Ordena do menor para o maior (padrão).
`DESC` (Descendente): Ordena do maior para o menor.

---------
### UNION

Combina o resultado de duas ou mais consultas SELECT.

SELECT [coluna1], [coluna2] FROM [tabela1]
UNION
SELECT [coluna1], [coluna2] FROM [tabela2]

`UNION`     = Junta os resultados e remove duplicatas.
`UNION ALL` = Junta os resultados e mantém duplicatas (mais rápido).