--------------
### Tipo de Dados: Listas (L/R)

Listas são coleções ordenadas de strings. Você pode adicionar elementos no início ou no final da lista.

RPUSH [chave elemento1 elemento2]  = Adiciona um ou mais elementos ao final de uma lista.
Ex: RPUSH fila:pedidos pedidos:001 pedidos:002 pedido:003 pedido:004

LPUSH [chave elemento1 elemento2]  = Adiciona um ou mais elementos ao início de uma lista.
Ex: LPUSH fila:pedidos pedidos:001 pedidos:002 pedido:003 pedido:004

LRANGE [chave start stop]          = Exibe um intervalo de elementos de uma lista.
0: Primeiro elemento.
-1: Último elemento.
Ex: LRANGE fila:pedidos 0 -1 (exibe todos os elementos do começo ao fim).

LPOP [chave]                       = Remove e retorna o elemento no início da lista.
RPOP [chave]                       = Remove e retorna o ultimo elemento da lista.

LPOP [chave quantidade]            = Remove e retorna a [quantidade] de elementos no inicio 
RPOP [chave quantidade]            = Remove e retorna a [quantidade] de elementos no fim

LLEN [chave]                       = Retorna a quantidade de elementos em uma lista.

LTRIM [chave start stop]           = Mantém apenas os elementos dentro do intervalo.
Ex: LTRIM fila:pedidos 1 2 (mantém os itens que estão entre as posições 1 e 2).

LINDEX [chave posição]             = Retorna o elemento em uma posição específica da lista.
Ex: LINDEX fila:pedidos 1 (retorna o segundo elemento da lista).

LREM [chave count] "[valor]":      = Remove ocorrências de um [valor] de uma lista.

2: Remove os primeiros [2] elementos do inicio ao fim.
"-" 2: Remove os últimos [2] elementos do [valor] do fim ao inicio.
0: Remove todas as ocorrências do [valor].

LSET [chave index] "[novo_valor]"  = Substitui o elemento na posição [index] da lista.

----------
### Tipo de Dados: Sets (S)
Sets são coleções não ordenadas de strings únicas. Não permitem elementos duplicados.

SADD [chave] "[membro1]" "[membro2]"   = Adiciona um ou mais membros a um set.
Ex: SADD colaborador:101:skills "Python" "SQL" "MongoDB"

SMEMBERS [chave]                       = Lista todos os membros de um set.
SISMEMBER [chave] "[membro]"           = Verifica se um membro existe em um set.
SINTER [chave1 chave2]                 = Retorna a intercessão de membros nos sets.
SUNION [chave1 chave2]                 = Retorna a união de membros nos sets.
SDIFF [chave1 chave2]                  = Retorna os membros que estão somente na [chave1].
SREM [chave] "[membro1]" "[membro2]"   = Remove um ou mais membros de um set.
SRANDMEMBER [chave quantidade]         = Retorna um ou mais membros aleatórios de um set.
SPOP [chave]                           = Remove e retorna um membro aleatório de um set.
SCARD [chave]                          = Retorna a quantidade de membros em um set.

-------
