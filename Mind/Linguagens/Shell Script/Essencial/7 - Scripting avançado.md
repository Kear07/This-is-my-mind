-------------------
### Funções

Permitem agrupar comandos em blocos reutilizáveis.

```bash
#!/bin/bash

# Define a função (duas sintaxes comuns)
function saudar() {
    echo "Olá do 'function saudar'!"
}

# Ou a sintaxe preferida (mais simples):
cumprimentar() {
    echo "Olá, $1!" # $1 é o primeiro argumento passado para a função
}

# --- Execução ---

# Chama as funções
saudar
cumprimentar "Kear" # Passa "Kear" como argumento ($1)
```

----
### Argumentos de Linha de Comando

São as variáveis especiais que o Shell define quando seu script é executado.

- `$0`: O nome do próprio script (ex: `./meu_script.sh`).
- `$1`: O primeiro argumento.
- `$2`: O segundo argumento (e assim por diante).
- `$#`: O **número total** de argumentos passados.
- `$@`: Todos os argumentos, passados como uma lista de strings separadas (geralmente a forma preferida).
- `$*`: Todos os argumentos, passados como uma única string.

-----------
### Códigos de Saída (Exit Codes)

Todo comando, ao terminar, retorna um número (o "código de saída") para indicar se foi bem-sucedido ou não.

- `0`: Sucesso.
- `1` a `255`: Falha (o número indica o tipo do erro).

A variável especial `$?` armazena o código de saída do **último comando executado**.

```bash
# Executa um comando que funciona
ls /home
echo "Código de saída: $?" # Saída: 0

# Executa um comando que falha
ls /pasta_inexistente
echo "Código de saída: $?" # Saída: (um número diferente de 0, ex: 2)

# Você pode usar 'exit' para definir o código de saída do seu script
exit 1 # Indica que o script falhou
```

----------
### Boas Práticas (Set)

Coloque estes comandos no início dos seus scripts para torná-los mais seguros e fáceis de depurar.

```bash
#!/bin/bash
set -e
set -o pipefail
set -u
```

- `set -e`: **(Mais importante)** Faz o script parar **imediatamente** se qualquer comando falhar (retornar um código de saída diferente de 0). Evita que erros se acumulem.
- `set -o pipefail`: Faz com que um _pipeline_ (`|`) falhe se _qualquer_ comando no pipeline falhar (não apenas o último).
- `set -u`: Trata o uso de variáveis não definidas (vazias) como um erro. Ajuda a pegar typos (ex: `echo $NOME` em vez de `echo $NOME_USUARIO`).

----------
