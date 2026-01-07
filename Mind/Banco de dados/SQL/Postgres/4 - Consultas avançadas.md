------------------
### Common Table Expression (CTE)

Uma Common Table Expression (CTE) é um conjunto de resultados nomeado temporário que você pode referenciar dentro de uma única instrução SQL (SELECT, INSERT, UPDATE, DELETE).
Elas são úteis para quebrar consultas complexas em partes menores e mais legíveis.

-------
#### Sintaxe Básica

WITH nome_da_cte AS (
    -- Sua consulta para definir a CTE aqui
    SELECT coluna1, coluna2 FROM tabela
)
-- Agora você pode consultar a CTE
SELECT * FROM nome_da_cte WHERE alguma_condicao;

----
#### Sintaxe Avançada (Múltiplas CTEs)

WITH
cte1 AS (
    SELECT ...
),
cte2 AS (
    SELECT ... FROM cte1 -- Pode referenciar CTEs anteriores
)
SELECT * FROM cte2;

-------
#### Exemplo de Uso

-- Usando CTE para encontrar o total de vendas por categoria e, em seguida, ranquear as categorias.
WITH VendasPorCategoria AS (
    SELECT categoria, SUM(valor) as total_vendas
    FROM Vendas
    GROUP BY categoria
)
SELECT 
    categoria, 
    total_vendas,
    RANK() OVER (ORDER BY total_vendas DESC) as rank_categoria
FROM VendasPorCategoria;

---
### Window Functions (Funções de Janela)

Funções de Janela executam cálculos em um conjunto de linhas (uma "janela") relacionadas à linha atual. Diferente do `GROUP BY`, elas **não agrupam** as linhas, elas retornam um valor para cada linha original.

Toda Window Function usa a cláusula `OVER()`. É ela quem define a "janela" de dados que a função vai analisar.

`FUNÇÃO() OVER (PARTITION BY [coluna_grupo] ORDER BY [coluna_ordem])`

| Parte                   | Descrição                                                                                                                                                               |
| :---------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `FUNÇÃO()`              | A função que você quer usar (ex: `RANK()`, `SUM()`, `LAG()`).                                                                                                           |
| `OVER ()`               | Cláusula que define a janela.                                                                                                                                           |
| `PARTITION BY [coluna]` | **(Opcional)** Divide os dados em "grupos" (partições). É como um `GROUP BY` temporário, mas que não agrupa o resultado final. Se omitido, a janela é a tabela inteira. |
| `ORDER BY [coluna]`     | **(Obrigatório para ranks/lag)** Define a ordem das linhas *dentro* de cada partição.                                                                                   |

---
#### Funções de Rank

Usadas para criar rankings.

| Função | Descrição (em caso de empates) | Exemplo de Ranking |
| :--- | :--- | :--- |
| `ROW_NUMBER()` | Atribui um número sequencial único. **Ignora empates**. | 1, 2, 3, 4 |
| `RANK()` | Dá o mesmo rank para empates e **PULA** posições. | 1, 2, 2, 4 |
| `DENSE_RANK()`| Dá o mesmo rank para empates e **NÃO PULA** posições. | 1, 2, 2, 3 |

**Exemplo:** Rankear produtos por valor, dentro de cada categoria.

```sql
SELECT id, produto, categoria, valor,
    -- Rankeia os produtos por categoria, do mais caro para o mais barato
    RANK() OVER (PARTITION BY categoria ORDER BY valor DESC) as rank_por_categoria
FROM Vendas;
```

--------------

#### Funções de Deslocamento (LAG / LEAD)

Usadas para "espiar" valores de linhas vizinhas (anterior ou próxima).

- `LAG(coluna, N, padrao)`: Acessa o valor da `coluna` que está `N` linhas **atrás** (anterior).
- `LEAD(coluna, N, padrao)`: Acessa o valor da `coluna` que está `N` linhas **à frente** (próxima).

**Parâmetros:**

1. **coluna**: A coluna que você quer "espiar".
2. **N**: (Opcional, padrão 1) Quantas linhas olhar para trás/frente.
3. **padrao**: (Opcional, padrão NULL) O que retornar se não houver linha (ex: na primeira linha, `LAG` não tem linha anterior).
4. 
Exemplo: Ver o valor da venda anterior do mesmo produto.

SELECT id, produto, valor, data_venda,
    -- Pega o 'valor' da linha anterior (offset 1), 
    -- com base na ordem de data_venda, 
    -- agrupado por produto.
    LAG(valor, 1, 0.00) OVER (PARTITION BY produto ORDER BY data_venda) as valor_venda_anterior
FROM Vendas;

---------
#### Funções de Posição (FIRST / LAST)

Pega o valor da primeira ou última linha da partição.

- `FIRST_VALUE(coluna) OVER (...)`
- `LAST_VALUE(coluna) OVER (...)`

**CUIDADO com `LAST_VALUE`!** Por padrão, a "janela" vai da primeira linha da partição até a _linha atual_. Isso significa que `LAST_VALUE` (por padrão) sempre retornará o valor da linha atual.
Para fazer o `LAST_VALUE` enxergar a partição inteira, você deve especificar o "frame" da janela manualmente:

`ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` (Linhas entre "sem limite anterior" e "sem limite seguinte")

**Exemplo:** Mostrar o maior e o menor valor da categoria em todas as linhas.

SELECT
    id, produto, categoria, valor,

    -- Pega o 'valor' da primeira linha (ordenado por valor DESC) da partição 'categoria'
    FIRST_VALUE(valor) OVER (PARTITION BY categoria ORDER BY valor DESC) as maior_valor_na_categoria,
    
    -- Pega o 'valor' da última linha (ordenado por valor DESC) da partição 'categoria'
    -- (Obrigatório usar o ROWS BETWEEN... para funcionar como esperado)
    LAST_VALUE(valor) OVER (
        PARTITION BY categoria 
        ORDER BY valor DESC 
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) as menor_valor_na_categoria
FROM Vendas;

