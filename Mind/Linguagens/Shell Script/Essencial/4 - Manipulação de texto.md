---------------

Ferramentas usadas para filtrar, fatiar, ordenar e transformar texto. Elas são quase sempre usadas em conjunto com pipes (`|`).
### grep (Global Regular Expression Print)

Busca por linhas que contêm um padrão (texto ou expressão regular).

```bash
# Procura "texto" dentro do arquivo
grep "texto" arquivo.txt

# Procura com número de linha (como no seu Comandos.md)
grep -n "texto" arquivo.txt

# Procura ignorando maiúsculas/minúsculas
grep -i "texto" arquivo.txt

# Procura por linhas que NÃO contêm o padrão (Inverte a busca)
grep -v "erro" log.txt

# Usando com pipe:
history | grep "docker"
```

-----
### head e tail

Mostram o início ou o fim de um arquivo.

```bash
# Mostra as 10 primeiras linhas (padrão)
head arquivo.txt

# Mostra as 3 primeiras linhas
head -n 3 arquivo.txt

# Mostra as 10 últimas linhas (padrão)
tail arquivo.txt

# Mostra as 3 últimas linhas (como no seu Comandos.md)
tail -n 3 /etc/group

# (O MAIS ÚTIL) Segue o arquivo em tempo real (ótimo para logs)
tail -f /var/log/syslog
```

----
### sort

Ordena as linhas de um arquivo.

```bash
# Ordena alfabeticamente
sort nomes.txt

# Ordena numericamente
sort -n numeros.txt

# Ordena em ordem reversa (decrescente)
sort -r nomes.txt
```

------
### uniq

Remove linhas duplicadas. **IMPORTANTE:** `uniq` só funciona em linhas **adjacentes** (vizinhas). Por isso, ele é quase sempre usado DEPOIS do `sort`.

```bash
# 1. Ordena o arquivo
# 2. Remove as duplicatas
sort nomes.txt | uniq

# Conta quantas vezes cada linha aparece
sort nomes.txt | uniq -c
```

-------
### cut

Corta e extrai "colunas" (campos) de um texto.

```bash
# Ex: Arquivo CSV separado por vírgula (,)
# -d',' -> Define o delimitador (vírgula)
# -f1   -> Pega o primeiro campo (coluna)

cat arquivo.csv | cut -d',' -f1

# Pega os usuários do sistema (delimitador ':')
cat /etc/passwd | cut -d':' -f1
```

-----
### sed (Stream Editor)

Usado para "procurar e substituir" (Find/Replace) em um fluxo de texto.

```bash
# Sintaxe: 's/antigo/novo/g'
# s = substituir
# g = global (substitui todas na linha, não só a primeira)

# Substitui "erro" por "AVISO" no arquivo
sed 's/erro/AVISO/g' log.txt

# Usando com pipe
echo "meu nome é joão" | sed 's/joão/kear/g'
# Saída: meu nome é kear
```

-------
