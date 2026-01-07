------------
### Tipo de Dados: Sorted Sets (Z)
Sorted Sets são coleções de membros únicos, onde cada membro está associado a uma pontuação (score) numérica. Os membros são armazenados em ordem crescente de score.

ZADD [chave score] "[membro]"      =  Adiciona um membro com um score a um sorted set.
Ex: ZADD ranking_vendedores 1500 "Hugo"

NX: Adiciona o membro somente se ele não existir.
Ex: ZADD ranking_vendedores NX 1800 "Bruna"
XX: Atualiza o score do membro somente se ele já existir.
CH: Retorna o número de membros que foram adicionados ou tiveram seus scores alterados 
INCR: Incrementa o score do membro pelo valor fornecido.
GT: Atualiza o score do membro somente se o novo score for maior que o score atual.
LT: Atualiza o score do membro somente se o novo score for menor que o score atual.

ZSCORE [chave] "[membro]"          = Exibe o score de um membro específico.

ZRANK [chave] "[membro]"           = Exibe o rank de um membro em ordem crescente de score.
ZREVRANK [chave] "[membro]"        = Exibe o rank de um membro em ordem decrescente de score.

ZREM [chave] "[membro]"            = Remove um membro.
ZREMRANGEBYSCORE [chave min max]   = Remove todos os membros cujo score está dentro do intervalo.
ZREMRANGEBYRANK [chave start stop] = Remove membros pelo seu rank.

ZCARD [chave]                      = Exibe a quantidade de membros.
ZRANGE [chave start stop]          = Exibe um intervalo de membros em ordem crescente de score.
0: Primeiro membro.
-1: Último membro.

ZRANGE [key start stop] WITHSCORES  = Exibe um intervalo de membros e os scores em ordem crescente.
ZRANGE [key start stop] REV WITHSCORES = Exibe um intervalo de membros e seus scores decrescente.

--------
### Tipo de Dados: Bitmaps
Bitmaps são uma forma eficiente de armazenar e manipular arrays de bits. Cada bit pode ser 0 ou 1.

SETBIT [chave offset valor]       = Seta o bit na posição offset (índice) como 0 ou 1.
Ex: SETBIT users:active 10 1      = Marca que o usuário com ID 10 está ativo.

GETBIT [chave offset]	      = Exibe o bit na posição offset.
Ex: GETBIT users:active 10    = Retorna 1 (usuário 10 está ativo).

BITCOUNT [chave]	          = Conta quantos bits estão em 1 
Ex: BITCOUNT users:active:    = Conta quantos bits estão em 1 

BITOP [operação destino chave1 chave2]  = Realiza operações bit a bit entre múltiplas chaves (AND, OR, XOR, NOT). O resultado é salvo em destino.

BITPOS [chave bit]	    = Encontra a posição do primeiro bit com o valor indicado (0 ou 1).

STRLEN [chave]	        = Retorna o tamanho da string armazenada na chave 

------