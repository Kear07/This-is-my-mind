-----------
### Chaves com Tempo de Expiração (TTL)

Você pode definir um tempo de vida para suas chaves, após certo tempo, elas se excluirão.

SET [chave] "[valor]" EX [2] =  Define uma chave-valor com uma duração de [2] segundos.
TTL [chave]                  =  Exibe o tempo restante de uma chave em segundos.

Retorno -1: A chave existe, mas não tem tempo de duração definido
Retorno -2: A chave não existe.

---
### Manipulação de Chaves (SET Avançado)

SET [chave] "[valor]" XX   = Altera o valor da chave somente se ela já existir.
SET [chave] "[valor]" NX   = Cria a chave somente se ela não existir.
GETSET [chave] "[valor]"  = Retorna o valor atual da chave e, atualiza o seu valor.

SCAN 0 MATCH [padrão] COUNT [num] = Itera as chaves do banco de dados.
0                                 = Ordem/index onde vai começar a busca
MATCH [padrão]                    = Filtra as chaves (mesmos padrões de KEYS).
COUNT [numero]                    = Número de elementos a serem retornados por cada iteração.

----
### Comandos para Múltiplas Chaves (M)

MSET [chave1] "[valor1]" [chave2] "[valor2]"  = Adiciona ou atualiza múltiplas chaves de uma vez.
MGET [chave1 chave2]                          = Exibe os valores de múltiplas chaves.

Observação: EX para expiração só funciona diretamente com SET. Para MSET, você precisaria definir a expiração para cada chave individualmente.

--------
### Tipo de Dados: Hashes (H)

Hashes são como objetos ou dicionários, permitindo armazenar múltiplos campos e valores dentro de uma única chave.

HSET [key campo1] "[valor1]" [campo2] "[valor2]"   = Cria ou atualiza campos dentro de um hash.
Ex: HSET user:123 email "marcelo@gmail.com"  
EX: HSET user:123 idade 33 
(adiciona múltiplos campos à chave user:123)

HMSET [key campo1] "[valor1]" [campo2] "[valor2]"  = Adiciona ou atualiza múltiplos campos
Ex: HMSET user:123 email "marcelo@gmail.com" idade 33

HGET [chave campo]  = Visualiza o valor de um campo específico dentro de um hash.
Ex: HGET user:123 email

HDEL [chave campo]  = Deleta um campo de um hash.
Ex: HDEL user:123 email

HGETALL [chave]: Exibe todos os campos e valores de um hash.
Ex: HGETALL user:123

---------