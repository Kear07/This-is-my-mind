-----------
### Gerenciamento de Permissões

No Linux, tudo é um arquivo, e todo arquivo tem permissões para três entidades:
* **U (User/Dono):** O proprietário do arquivo.
* **G (Group/Grupo):** O grupo ao qual o arquivo pertence.
* **O (Others/Outros):** Todos os demais usuários.

As permissões são:
* **R (Read/Ler):** `4`
* **W (Write/Modificar):** `2`
* **X (Execute/Executar):** `1`

-------
### chmod (Change Mode)
Altera as permissões de um arquivo ou pasta.

#### Método 1: Numérico (Octal)
Este é o método mais comum. Você soma os valores (R=4, W=2, X=1) para cada entidade (Dono, Grupo, Outros).

`rwx r-x r-x  ->  (4+2+1) (4+0+1) (4+0+1)  ->  755`

![Permissões Padrão](permissões%20especiais.png)

```bash
# chmod [Dono][Grupo][Outros] [arquivo]

# Permissão total para o Dono, leitura/execução para Grupo e Outros
# (Comum para scripts e pastas)
chmod 755 meu_script.sh

# Permissão de leitura/escrita para Dono, e apenas leitura para os demais
# (Comum para arquivos de texto)
chmod 644 config.txt
```

#### Método 2: Simbólico (Letras)

Permite adicionar ou remover permissões específicas.

- `u, g, o, a` (user, group, other, all)
- `+` (Adicionar permissão)
- `-` (Remover permissão)
- `r, w, x` (read, write, execute)

```bash
# Adiciona a permissão de execução (x) para o Dono (u)
chmod u+x meu_script.sh

# Remove a permissão de escrita (w) do Grupo (g) e Outros (o)
chmod go-w dados_sensivies.txt
```

-------
### Permissões Especiais (Sticky bit, SUID, SGID)

São um dígito extra (ex: `chmod 4755`).

### chown (Change Owner)

Altera o dono e/ou o grupo de um arquivo/pasta.

```bash
# Altera o Dono do arquivo
chown aluno arquivo.txt

# Altera o Grupo do arquivo (note os dois pontos)
chown :grupo arquivo.txt

# Altera o Dono E o Grupo de uma vez
chown aluno:grupo arquivo.txt
```

----------
### Gerenciamento de Processos

#### ps (Process Status)

Tira uma "foto" dos processos que estão rodando no momento.

```bash
# Exibe todos os processos em detalhes (Formato BSD)
ps aux

# Exibe todos os processos (Formato SystemV, similar ao seu ps -all)
ps -ef

# O mais útil: ps + grep para achar um processo específico
ps aux | grep "chrome"
```

#### top (Table of Processes)

Um monitor de processos em tempo real (como o Gerenciador de Tarefas). Mostra uso de CPU, memória, etc.

```bash
top
# (Pressione 'q' para sair)
```

#### kill (Parar Processos)

Envia um sinal para um processo (geralmente para encerrá-lo).

```bash
# 1. Encontre o PID (Process ID) do processo
# ps aux | grep "nome_processo"

# 2. Envie o sinal de término (SIGTERM, 15 - o "jeito educado")
kill [PID]

# 3. Se o processo não fechar, force o encerramento (SIGKILL, 9)
kill -9 [PID]
```

-------

