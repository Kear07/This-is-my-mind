-------------------------------

No Linux, todo processo tem três "canais" de comunicação:
1.  **stdin (0):** Entrada padrão (geralmente o teclado).
2.  **stdout (1):** Saída padrão (geralmente a tela/terminal).
3.  **stderr (2):** Saída de erro (geralmente a tela/terminal).

Redirecionamento e Pipes são formas de controlar e "plugar" esses canais.
### Redirecionamento de Saída (stdout)

Controla o que fazer com a *saída normal* de um comando (o resultado esperado).

#### `>` (Sobrescrever)
Pega a saída do comando e a **sobrescreve** em um arquivo. Se o arquivo não existir, ele é criado.

```bash
# Salva a data atual no arquivo, apagando o que estava antes
date > data.txt

# Cria um arquivo novo e espera você digitar o conteúdo.
# (Pressione Ctrl+D para salvar e sair)
cat > arquivo.txt
```

#### `>>` (Anexar / Append)

Pega a saída do comando e a **adiciona ao final** de um arquivo. Se o arquivo não existir, ele é criado.

```bash
# Adiciona a data atual ao final do arquivo, sem apagar o conteúdo
date >> data.txt

# Adiciona mais texto ao final de um arquivo existente
cat >> arquivo.txt
# (Pressione Ctrl+D para salvar e sair)
```

#### O Pipe (`|`)

O "cano" é o operador mais poderoso. Ele pega a **saída (stdout)** do `comando1` e a usa como **entrada (stdin)** para o `comando2`.

**Sintaxe:** `comando1 | comando2`

```bash
# Pega a lista de todos os processos em execução...
# ...e "filtra" (grep) apenas as linhas que contêm a palavra "chrome"
ps aux | grep "chrome"

# Pega o histórico de comandos...
# ...e "filtra" (grep) apenas os comandos que usaram "ls"
history | grep "ls"
```

----------
### Redirecionamento de Erro (stderr)

Controla o que fazer com as _mensagens de erro_ (a saída `stderr`, ou canal `2`).

#### `2>` (Redirecionar Erros)

Pega apenas as mensagens de erro (canal `2`) e as envia para um arquivo.

```bash
# Tenta acessar uma pasta que não existe.
# O erro "No such file or directory" será salvo em 'log_de_erros.txt'
# A saída normal (se houvesse) iria para a tela.
ls /pasta_inexistente 2> log_de_erros.txt
```

#### `2> /dev/null` (Descartar Erros)

A forma mais comum de usar. Envia os erros para o "buraco negro" do Linux, silenciando o comando.

```bash
# Executa o 'find' e ignora todas as mensagens de "Permissão negada"
find / -name "kear.txt" 2> /dev/null
```

#### `&>` (Redirecionar Tudo: stdout e stderr)

Uma forma curta (atalho do Bash) para enviar a saída normal **E** os erros para o mesmo arquivo.

```bash
# Salva TUDO (saída e erros) do comando no arquivo 'log_completo.txt'
apt-get update &> log_completo.txt
```

----------
