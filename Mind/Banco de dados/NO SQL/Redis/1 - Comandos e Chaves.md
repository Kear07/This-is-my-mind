----------
### Inicialização
 
docker exec -it redis redis-cli           = Inicia a interface de linha de comando do Redis 

----
### Comandos básicos

KEYS *                = Exibe todas as chaves armazenadas no banco.
KEYS "*-05*"            = Exibe chaves que contêm "-05-" em qualquer posição.
KEYS *-15-20*:          = Exibe chaves que contêm "-15-20-" em qualquer posição.
KEYS user:?:name      = Substitui um único caractere (coringa de um caractere). 
KEYS user:            = Substitui zero ou mais caracteres (coringa de múltiplos caracteres).  
KEYS user:[abc]       = Lista de caracteres. Ex: (user:a, user:b, user:c).
KEYS user:[^e]         = Negação de letra. Ex: (user:a, user:b, mas não user:e).
KEYS user\            = Caractere de escape para usar caracteres especiais.

DBSIZE                = Exibe o número de chaves existentes no banco.
EXISTS [chave]        = Verifica se uma chave existe.
DEL [chave]           = Deleta uma chave. 
UNLINK [chave]        = Deleta uma ou mais chaves de forma assíncrona.
RENAME [antiga nova]  = Renomeia uma chave.
FLUSHALL              = Limpa todos os bancos de dados.

---
### Gerenciamento de Bancos de Dados

O Redis pode ter múltiplos bancos de dados, numerados de 0 a 15 por padrão.

config get databases   = Exibe a quantidade de bancos de dados configurados.
SELECT [0-15]          = Troca para o banco de dados especificado.

---------