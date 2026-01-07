------------

Para escrever scripts, você precisa controlar o fluxo de execução com condicionais (if/case) e loops (for/while).

### Condicional `if`

A sintaxe `if` no Bash é verbosa e usa `[ ... ]` (que é um atalho para o comando `test`) para avaliar condições.

**Importante:** Os **espaços** dentro e ao redor dos colchetes `[ ]` são **obrigatórios**.

```bash
#!/bin/bash

read -p "Digite sua idade: " IDADE

if [ "$IDADE" -gt 18 ]; then
    echo "Você é maior de idade."
elif [ "$IDADE" -eq 18 ]; then
    echo "Você tem exatamente 18 anos."
else
    echo "Você é menor de idade."
fi
# 'fi' fecha o bloco 'if'
```

------------
### Operadores de Comparação (dentro de `[ ]`)

**NÃO** use `==` ou `>` para números. Use os operadores de texto abaixo:

- **Para Números:**
    
    - `-eq`: (Equal) Igual 
    - `-ne`: (Not Equal) Diferente
    - `-gt`: (Greater Than) Maior que
    - `-ge`: (Greater or Equal) Maior ou igual a
    - `-lt`: (Less Than) Menor que
    - `-le`: (Less or Equal) Menor ou igual a

- **Para Strings:**
    
    - `"$VAR1" == "$VAR2"`: Igual (use aspas!)  
    - `"$VAR1" != "$VAR2"`: Diferente
    - `-z "$VAR"`: Verifica se a string é nula/vazia.
    
- **Para Arquivos:**
    
    - `-f "/path/arq.txt"`: Verifica se é um arquivo. 
    - `-d "/path/pasta"`: Verifica se é um diretório.
    - `-e "/path/algo"`: Verifica se existe (arquivo OU diretório).
    

--------------
### Condicional `case`

Uma alternativa mais limpa ao `if` para verificar um valor contra múltiplos padrões (como o `switch` em Java).

```bash
read -p "Digite 'start', 'stop' ou 'status': " COMANDO

case "$COMANDO" in
    start)
        echo "Iniciando o serviço..."
        ;;
    stop)
        echo "Parando o serviço..."
        ;;
    status|stats) # Permite múltiplos padrões (OU)
        echo "Verificando o status..."
        ;;
    *) # Padrão 'default' (pega qualquer outra coisa)
        echo "Comando inválido."
        ;;
esac
# 'esac' fecha o bloco 'case' (case ao contrário)
```

--------
### Loop `for`

Usado para iterar sobre uma lista de itens.

```bash
# Itera sobre uma lista de strings
echo "Iterando sobre frutas:"
for FRUTA in "Maçã" "Banana" "Uva"; do
    echo "Eu gosto de $FRUTA"
done

# Itera sobre arquivos no diretório
echo -e "\nArquivos .txt:"
for ARQUIVO in *.txt; do
    echo "Encontrado: $ARQUIVO"
done

# Loop 'C-Style' (para contagem)
echo -e "\nContagem:"
for (( i=0; i<5; i++ )); do
    echo "Número: $i"
done
```

------
### Loop `while`

Executa um bloco de código _enquanto_ uma condição for verdadeira.

```bash
# Exemplo de contador
CONTADOR=0
while [ $CONTADOR -lt 5 ]; do
    echo "Contagem: $CONTADOR"
    # Matemática no Shell é feita com $((...))
    CONTADOR=$((CONTADOR + 1)) 
done
```

------
