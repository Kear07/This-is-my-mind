----------------
### Conceitos Básicos do PL/pgSQL

PL/pgSQL é a linguagem de programação procedural nativa do PostgreSQL.
Ela permite criar blocos de código (como funções, procedures e triggers) que rodam *dentro* do banco de dados.

Quase todo código PL/pgSQL vive dentro desta estrutura:

```sql
-- A declaração da função/procedure/trigger vem aqui
RETURNS ... AS $$
DECLARE
    -- (Opcional) Declaração de variáveis
    contador INT := 0;
    nome_cliente VARCHAR;
BEGIN
    -- Lógica principal do bloco
    
    IF contador = 0 THEN
        RAISE NOTICE 'O contador é zero';
    END IF;

    -- Lógica...
    
EXCEPTION
    -- (Opcional) Bloco para tratamento de erros
    WHEN OTHERS THEN
        RAISE EXCEPTION 'Um erro inesperado ocorreu';
END;
$$ LANGUAGE plpgsql;
```

#### Declaração de Variáveis (DECLARE)

Variáveis são declaradas na seção `DECLARE`.

```sql
DECLARE
    id_usuario INT;
    email_usuario VARCHAR(100);
    preco_final NUMERIC(10, 2) := 0.0; -- Atribuição de valor padrão
```

#### SELECT INTO

Para guardar o resultado de uma query em uma variável, use `SELECT ... INTO ...`.

```sql
SELECT nome, email INTO nome_cliente, email_usuario
FROM clientes WHERE id = id_usuario;
```

Controle de Fluxo (IF)

```sql
IF condicao THEN
    -- Código se verdadeiro
ELSEIF outra_condicao THEN
    -- Código se verdadeiro
ELSE
    -- Código se falso
END IF;
```

----------------------
### Funções (Functions)

São blocos de PL/pgSQL que **executam uma lógica e retornam um resultado**. Elas são chamadas usando `SELECT`.

```sql
CREATE OR REPLACE FUNCTION nome_da_funcao (parametro1 tipo, ...)
RETURNS tipo_de_retorno AS $$
DECLARE
    -- Variáveis
BEGIN
    -- Lógica
    RETURN valor_ou_tabela; 
END;
$$ LANGUAGE plpgsql;
```

**Comandos:**

- `select * from [nome()]` = Chama uma função (que retorna tabela)
- `select [nome()]` = Chama uma função (que retorna valor único)
- `drop function [nome()]` = Deleta uma função

**Tipos de Retorno:**

- `RETURNS INT`, `RETURNS VARCHAR`, etc: Retorna um tipo/variável simples.
- `RETURNS void`: Sem retorno (raro, prefira Procedure).
- `RETURNS TABLE(...)`: Retorna um conjunto de resultados (uma tabela).
- `RETURNS SETOF [tipo]`: Retorna uma coluna/tabela de um tipo.
- `RETURNS record`: Retorna uma linha com colunas indefinidas.

#### Exemplos de Funções

```sql
-- FUNÇÃO 1: Retorna um valor único (INT)
CREATE OR REPLACE FUNCTION total_livro_estoque()
RETURNS INT AS $$
DECLARE
    total INT;
BEGIN
    SELECT SUM(estoque) INTO total FROM livros;
    -- COALESCE(total, 0) = Retorna o total, se for null, retorna 0
    RETURN COALESCE(total, 0);
END;
$$ LANGUAGE plpgsql;
```

```sql
-- FUNÇÃO 2: Retorna uma tabela (TABLE)
CREATE OR REPLACE FUNCTION listar_autores_livros()
RETURNS TABLE (autor VARCHAR, quantidade_livros BIGINT) AS $$
BEGIN
    -- RETURN QUERY executa a query e retorna o resultado
    RETURN QUERY
    SELECT a.nome, COUNT(la.livro_id)
    FROM autores a
    LEFT JOIN livrosautores la ON la.autor_id = a.id
    GROUP BY a.nome
    ORDER BY COUNT(la.livro_id) DESC;
END;
$$ LANGUAGE plpgsql;
```

--------
### Procedures

São blocos de PL/pgSQL muito similares às funções, mas com duas diferenças:

1. **Não retornam um valor** (são `RETURNS void`).
2. São chamadas usando `CALL`, não `SELECT`.

Elas são ideais para executar _ações_ (INSERTS, UPDATES, DELETES) que não precisam de um retorno.

**Comandos:**

- `call [nome()]` = Chama/Executa a procedure
- `drop procedure [nome()]` = Deleta uma procedure
- 
#### Exemplo de Procedure

```sql
-- PROCEDURE: Adicionar ao estoque
CREATE OR REPLACE PROCEDURE adicionar_estoque(titulo_livro VARCHAR, quantidade INT)
LANGUAGE plpgsql AS $$
DECLARE
    livro_id INT;
BEGIN
    IF quantidade <= 0 THEN
        -- Interrompe a execução e reporta um erro
        RAISE EXCEPTION 'A quantidade deve ser positiva.';
    END IF;

    SELECT id INTO livro_id FROM livros WHERE titulo = titulo_livro;
    IF NOT FOUND THEN
        RAISE EXCEPTION 'Livro não encontrado.';
    END IF;

    UPDATE livros SET estoque = estoque + quantidade WHERE id = livro_id;
    
    -- Envia um aviso para o console
    RAISE NOTICE 'Estoque atualizado com sucesso!';
END;
$$;
```

--------------------------
### Triggers

Uma Trigger (ou gatilho) é um mecanismo que **executa uma função** automaticamente em resposta a um evento (INSERT, UPDATE, DELETE) em uma tabela.

**O processo tem 2 passos:**

1. **Criar uma Função** que retorna o tipo especial `TRIGGER`.
2. **Anexar (bind) a Função** a uma tabela usando `CREATE TRIGGER`.

**Comandos:**

- `drop trigger [trigger] on [table]` = Exclui uma trigger
- `alter table [table] disable trigger [trigger]` = Desativa uma trigger
- `alter table [table] enable trigger [trigger]` = Ativa uma trigger
#### Variáveis Especiais (OLD e NEW)

Dentro de uma função de Trigger, você tem acesso a:

- `OLD`: Contém a linha _antes_ da operação (Disponível em `UPDATE` e `DELETE`).
- `NEW`: Contém a linha _depois_ da operação (Disponível em `INSERT` e `UPDATE`).

-----------------
#### Exemplo 1: BEFORE INSERT (para padronizar dados)

**Passo 1: A Função**
```sql
CREATE OR REPLACE FUNCTION padronizar_usuario()
RETURNS TRIGGER AS $$
BEGIN
    -- Converte o email para minúsculas ANTES de inserir
    NEW.email := LOWER(NEW.email);

    -- Se data_cadastro não foi fornecida, preenche com a data/hora atual
    IF NEW.data_cadastro IS NULL THEN
        NEW.data_cadastro := NOW();
    END IF;

    -- É CRUCIAL retornar NEW para triggers BEFORE
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

Passo 2: O Trigger
```sql
CREATE TRIGGER trigger_usuario
BEFORE INSERT ON usuarios           -- Dispara ANTES de cada INSERÇÃO
FOR EACH ROW                        -- Para cada linha que está sendo inserida
EXECUTE FUNCTION padronizar_usuario();
```

------------------------------
#### Exemplo 2: AFTER UPDATE (para criar logs)

**Passo 1: A Função**

```sql
CREATE OR REPLACE FUNCTION log_alteracao_preco()
RETURNS TRIGGER AS $$
BEGIN
    -- Verifica se o preço realmente mudou
    IF OLD.preco IS DISTINCT FROM NEW.preco THEN 
        INSERT INTO historico_precos (
            produto_id, preco_antigo, preco_novo, data_alteracao
        ) VALUES (
            NEW.id, OLD.preco, NEW.preco, NOW()
        );
    END IF;
    RETURN NEW; -- Em triggers AFTER, o retorno não é tão crítico, mas é boa prática
END;
$$ LANGUAGE plpgsql
```

Passo 2: O Trigger
```sql
CREATE TRIGGER trig_preco
AFTER UPDATE ON produtos            -- Dispara DEPOIS de um UPDATE
FOR EACH ROW
EXECUTE FUNCTION log_alteracao_preco();
```

---------------
### Tratamento de Erros (RAISE)

`RAISE` é usado para enviar mensagens ou interromper a execução do bloco PL/pgSQL.

|**Comando**|**Descrição**|
|---|---|
|`RAISE EXCEPTION [msg]`|**Interrompe a transação** e reporta um erro fatal.|
|`RAISE NOTICE [msg]`|Envia uma mensagem de aviso ao cliente (console).|
|`RAISE WARNING [msg]`|Mensagem de problema que não é fatal.|
|`RAISE INFO [msg]`|Mensagem informativa.|
|`RAISE LOG [msg]`|Mensagem de log (geralmente salva no log do servidor).|
|`RAISE DEBUG [msg]`|Mensagem de depuração.
