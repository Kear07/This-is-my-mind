-------------------
### O "Shebang"
Todo script shell deve começar com esta linha. Ela diz ao sistema qual interpretador usar.

```bash
#!/bin/bash
```

-------
### Saída (Output)

O comando `echo` é usado para imprimir texto no terminal.

```bash
# Imprime o texto
echo "Olá, mundo!"

# Opções comuns:
# -n : Não pula a linha no final
echo -n "Meu nome é "
echo "Kear."
# Saída: Meu nome é Kear.

# \n : Pula uma linha (requer a flag -e)
echo -e "Linha 1\nLinha 2"
```

-----------------------
### Variáveis

Em shell, variáveis são declaradas sem espaços ao redor do `=`. Para ler o valor, use `$`.

```bash
# Declaração (SEM espaços)
NOME="Kear_07"
IDADE=25

# Uso (com $)
echo "Meu nome é $NOME e eu tenho $IDADE anos."

# Concatenação (use {} para delimitar)
echo "Eu sou o ${NOME}."
```

------
### Input do Usuário (Entrada)

O comando `read` pausa o script e espera o usuário digitar, salvando a entrada em uma variável.

```bash
# Pergunta ao usuário
echo "Qual é o seu nome?"

# Lê a entrada e salva na variável 'USUARIO'
read USUARIO

echo "Bem-vindo, $USUARIO!"

# Opção -p (prompt): Faz a pergunta na mesma linha
read -p "Qual sua linguagem favorita? " LINGUAGEM
echo "$LINGUAGEM é uma ótima escolha."
```

------------
### Aspas Simples vs. Aspas Duplas

Este é um conceito crucial no Shell.

- **Aspas Duplas (`"..."`)**: **Interpretam** variáveis e caracteres especiais.  
- **Aspas Simples (`'...'`)**: **Não interpretam nada.** Tratam tudo como texto literal.

```bash
USUARIO="Kear"

echo "Olá, $USUARIO!"  # Saída: Olá, Kear!
echo 'Olá, $USUARIO!'  # Saída: Olá, $USUARIO!
```

----------
