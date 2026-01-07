---------------------

Este arquivo cobre os comandos essenciais para se mover pelo sistema de arquivos e manipular arquivos/diretórios.

### Comandos de Navegação

```bash
pwd
# (Print Working Directory)
# Exibe o caminho absoluto do diretório em que você está.

ls
# (List)
# Exibe os arquivos e pastas do diretório atual.

ls -l
# (List - Long)
# Exibe uma lista detalhada (permissões, dono, tamanho, data).

ls -a
# (List - All)
# Exibe todos os arquivos, incluindo os ocultos (que começam com ".").

cd [caminho]
# (Change Directory)
# Muda para o diretório especificado.
# Ex: cd /home/kear/Documentos

cd ..
# Volta um nível (para o diretório "pai").

cd /
# Vai direto para o diretório raiz (o "início" do sistema).

cd
# (Sem argumentos) Volta para o seu diretório "home" (/home/kear).
```

-----------
### Comandos de Criação e Leitura

```bash
mkdir [nome_pasta]
# (Make Directory)
# Cria uma nova pasta (diretório).

touch [nome_arquivo]
# Cria um arquivo novo e vazio (ou atualiza a data de modificação se ele já existir).

cat [nome_arquivo]

# Exibe o conteúdo completo de um arquivo de texto no terminal.

less [nome_arquivo]
# Permite ler arquivos grandes paginando (use 'q' para sair).
```
(Nota: Os comandos `cat >` e `cat >>` são Redirecionamentos; eles estarão no próximo arquivo.)

-------------------
### Comandos de Manipulação (Mover, Copiar, Remover)

```bash
cp [origem] [destino]
# (Copy)
# Copia um arquivo para um novo local.
# Ex: cp /home/kear/arquivo.txt /media/pendrive/

cp -r [pasta_origem] [pasta_destino]
# (Copy - Recursive)
# Necessário para copiar pastas (diretórios) inteiras.

mv [origem] [destino]
# (Move)
# Move um arquivo ou pasta para um novo local.
# Ex: mv /home/kear/arquivo.txt /home/kear/Documentos/

mv [nome_antigo] [nome_novo]
# (Move/Rename)
# Se a origem e o destino estiverem no mesmo diretório, 'mv' renomeia o arquivo.
# Ex: mv relatorio.txt relatorio_final.txt

rm [nome_arquivo]
# (Remove)
# Deleta um arquivo permanentemente.

rm -r [nome_pasta]
# (Remove - Recursive)
# Deleta uma pasta e TUDO o que estiver dentro dela.
# CUIDADO: Este comando é permanente. Use com extrema cautela.
```

--------
### Comandos de Busca

```bash
find [caminho] -name "[padrão]"
# Procura por arquivos ou pastas.

# Procura arquivos que terminam com ".txt" a partir do diretório atual (.)
# Ex: find . -name "*.txt"

# Procura arquivos com o nome exato "Comandos.md" em todo o sistema (/)
# Ex: find / -name "Comandos.md"
```

---------
