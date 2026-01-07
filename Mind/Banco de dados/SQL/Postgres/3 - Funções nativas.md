---------------
### Funções de Data & Hora

current_date                                     = Exibe a data atual (YYYY-MM-DD)
current_time                                     = Exibe o horário atual (HH:MM:SS com fuso)
now()                                            = Exibe data e horário atual (completo com fuso)

show datestyle                                   = Exibe o modelo de data atual (ex: 'ISO, DMY')
set datestyle = 'ISO, DMY'                       = Altera o modelo de data para Dia/Mês/Ano
set datestyle = 'ISO, MDY'                       = Altera o modelo de data para Mês/Dia/Ano

---
#### Extração de Tempo (EXTRACT)

-- Padrão: select extract([unidade] from [coluna_data_hora]) 

select extract(year from now())                  = Extrai o Ano
select extract(month from now())                 = Extrai o Mês (1-12)
select extract(day from now())                   = Extrai o Dia (1-31)
select extract(hour from now())                  = Extrai a Hora (0-23)
select extract(minute from now())                = Extrai o Minuto (0-59)
select extract(second from now())                = Extrai o Segundo (0-59.99)
select extract(dow from now())                   = Extrai o Dia da Semana (0=Domingo, 6=Sábado)
select extract(epoch from now())                 = Total de segundos desde 1970-01-01 (Timestamp Unix)

---
#### Fuso Horário (TIMEZONE)

-- Exemplo para pegar o horário local de São Paulo formatado
SELECT 
    extract(hour from now() AT TIME ZONE 'America/Sao_Paulo') || ':' || 
    extract(minute from now() AT TIME ZONE 'America/Sao_Paulo') || ':' || 
    round(extract(second from now() AT TIME ZONE 'America/Sao_Paulo'), 0) AS "Horário Local" 

---------
### Funções Numéricas

select round([colun], 2) from [table]                = Arredonda em X casas decimais
select trunc([colun], 0) from [table]                = Remove (trunca) X casas decimais
select abs([colun]) from [table]                     = Exibe o valor absoluto (remove o sinal negativo)

select |/[colun] from [table]                        = Exibe a raiz quadrada (sqrt)
select ||/[colun] from [table]                       = Exibe a raiz cúbica (cbrt)

select power(9, 3)                                   = Exibe a potenciação (9 elevado a 3)

select ceil([colun]) from [table]                    = Arredonda para cima (retorna o menor inteiro >= ao valor)
select floor([colun]) from [table]                   = Arredonda para baixo (retorna o maior inteiro <= ao valor)

select random()                                      = Gera um número de 0.0 a 1.0
select (random() * 10 + 1)::int                      = Gera um número inteiro entre 1 e 10

---
### Funções de Texto

#### Básicas
select [text1] || [text2]                      = Concatena textos 
select char_length('Kear')                     = Exibe a quantidade de caracteres
select lower([text])                           = Transforma tudo em minúsculo 
select upper([text])                           = Transforma tudo em maiúsculo
select initcap('kear 07')                      = Inicia cada palavra com letra maiúscula (Resultado: 'Kear 07')
select substring('Maçã', 1, 3)                 = Extrai texto (da posição 1, pegue 3 caracteres). Resultado: 'Maç'
select reverse('Kear_07')                      = Inverte o texto 
select right('Kear_07', 2)                     = Exibe os últimos X caracteres. Resultado: '07'
select left('Kear_07', 4)                      = Exibe os primeiros X caracteres. Resultado: 'Kear'
select '4211'::int                             = Converte (cast) texto para inteiro 
select 4211::varchar                           = Converte (cast) inteiro para texto 

#### Limpeza e Formatação (Trim, Pad)
select trim('  texto   ')                          = Remove espaços do início E do fim.
select ltrim('  texto   ')                         = Remove espaços da esquerda (início).
select rtrim('  texto   ')                         = Remove espaços da direita (fim).
select trim(both 'x' from 'xxKearxx')              = Remove 'x' de ambos os lados. Resultado: 'Kear'
select trim(leading 'x' from 'xxKearxx')           = Remove 'x' do início. Resultado: 'Kearxx'
select trim(trailing 'x' from 'xxKearxx')          = Remove 'x' do fim. Resultado: 'xxKear'

select lpad('5', 3, '0')                           = Preenche à esquerda (L-eft) até o tamanho 3. Resultado: '005'
select rpad('Kear', 7, '_0')                       = Preenche à direita (R-ight) até o tamanho 7. Resultado: 'Kear_0'

#### Busca e Substituição
select replace('texto antigo', 'antigo', 'novo')   = Substitui uma substring. Resultado: 'texto novo'
select position('ata' in 'Batata')                 = Encontra a posição (iniciando em 1) da primeira ocorrência. Resultado: 2
select split_part('nome@dominio.com', '@', 1)      = Quebra o texto por um delimitador e pega a parte N. Resultado: 'nome'
select translate('Kear_07', 'K_07', 'k-o&')        = Substitui caracteres 1 a 1 (K->k, _->-, 0->o, 7->&). Resultado: 'kear-o&'

#### Comparação (LIKE, ILIKE)
-- LIKE é case-sensitive (diferencia maiúsculas/minúsculas)
-- ILIKE é case-insensitive (IGNORA maiúsculas/minúsculas)

select 'abc' like 'a%';        = Retorna true.
select 'abc' ilike 'A%';       = Retorna true. 

%                              = Corresponde a qualquer sequência de 0 ou mais caracteres.
_                              = Corresponde a qualquer caractere único.

-- Ex: 'Batata' like 'B%'      (Começa com B)
-- Ex: 'Batata' like '%a'      (Termina com a)
-- Ex: 'Batata' like '%at%'    (Contém 'at' em qualquer lugar)
-- Ex: 'Teto' like '_eto'      (Começa com 1 char, seguido de 'eto')

#### Expressões Regulares (Regex)
-- São mais poderosas que LIKE.

~                                                    = Match case-sensitive. Ex: 'abc' ~ 'a.*'
~* = Match case-insensitive. Ex: 'Abc' ~* 'a.*'
!~                                                   = Não match case-sensitive. Ex: 'abc' !~ 'd.*'
!~* = Não match case-insensitive. Ex: 'abc' !~* 'A.*'

-- Meta-caracteres comuns:
-- .    = Corresponde a um único caractere (equivale ao '_')
-- .* = Corresponde a zero ou mais caracteres (equivale ao '%')
-- ^    = Início da string. Ex: '^Kear' (Começa com 'Kear')
-- $    = Fim da string. Ex: '07$' (Termina com '07')
-- [ ]  = Lista de caracteres. Ex: '[a-c]to' (corresponde a 'ato', 'bto', 'cto')
-- |    = OU. Ex: '(B|b)atata' (corresponde a 'Batata' ou 'batata')
-- \d   = Um dígito (0-9)

----
### Funções de Agregação

-- Realizam um cálculo em um conjunto de linhas (geralmente com GROUP BY) e retornam um único valor.

COUNT()  = Conta o número de linhas ou valores não nulos.
SUM()	 = Calcula a soma total de uma coluna numérica.
AVG()	 = Calcula a média de uma coluna numérica.
MIN()	 = Encontra o menor valor em uma coluna.
MAX()	 = Encontra o maior valor em uma coluna.

Ex: 
SELECT COUNT(*) AS [total_pedidos] FROM pedidos     = Conta quantos pedidos existem