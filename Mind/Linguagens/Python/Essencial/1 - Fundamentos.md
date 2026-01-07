-------------
### Tipos de Dados Primitivos

Python é uma linguagem de tipagem dinâmica, o que significa que você não precisa declarar o tipo da variável.

```python
nome = 'Ana'          # str (String, texto)
idade = 23            # int (Inteiro)
salario = 200.89      # float (Ponto flutuante, decimal)
solteiro = True       # bool (Booleano, True ou False)
nenhum = None         # NoneType (Representa a ausência de valor)
```

--------------
### Comentários

Comentários são ignorados pelo interpretador e servem para deixar notas.

```python
# Este é um comentário de linha única.

"""
Esta é uma docstring,
usada para documentar funções, classes ou
módulos, mas também serve como comentário
de múltiplas linhas.
"""
```

-------
### Entrada (Input)

A função `input()` **sempre** retorna uma string (texto). Você precisa converter (fazer "cast") se quiser outro tipo.

```python
# input() sempre retorna uma string
nome = input("Digite seu nome: ")

# Convertendo a entrada para outros tipos
idade = int(input("Digite sua idade: "))
altura = float(input("Digite sua altura: "))
```

----
## Saída (Output)

A função `print()` exibe valores no console.

```python
print("Olá, mundo!")

# A forma mais moderna e recomendada: f-strings
print(f"Olá, {nome}. Você tem {idade} anos.")

# Forma antiga: .format()
print("Olá, {}. Você tem {} anos.".format(nome, idade))

# Concatenação simples (menos flexível)
print("Seu nome é " + nome)
```

---------
### Operadores

#### Aritméticos

- `+` : Soma
- `-` : Subtração
- `*` : Multiplicação
- `/` : Divisão (sempre retorna float)
- `//` : Divisão Inteira (descarta a parte decimal)
- `%` : Módulo (resto da divisão)
- `**` : Exponenciação (potência)
#### Atribuição

- `=` : Atribuição simples    
- `+=` : Adição ( `x += 1` é o mesmo que `x = x + 1`)
- `-=` : Subtração (`x -= 1` é o mesmo que `x = x - 1`)
- `*=` : Multiplicação
- `/=` : Divisão
- `//=` : Divisão Inteira
- `%=` : Módulo
- `**=` : Exponenciação
#### Comparação (Retornam True ou False)

- `==` : Igual a
- `!=` : Diferente de
- `>` : Maior que
- `<` : Menor que
- `>=` : Maior ou igual a
- `<=` : Menor ou igual a
#### Lógicos

- `and` : "E" (Ambos os lados devem ser `True`)
- `or` : "OU" (Pelo menos um lado deve ser `True`)
- `not` : "NÃO" (Inverte o valor booleano)
#### Identidade (Comparam a identidade do objeto na memória)

- `is` : "É" (Verifica se duas variáveis apontam para o _mesmo_ objeto na memória)
- `is not` : "Não é" (Verifica se _não_ são o mesmo objeto)

```python
a = [1, 2]
b = [1, 2]
c = a

print(a == b) # True (conteúdo é igual)
print(a is b) # False (são objetos diferentes na memória)
print(a is c) # True (apontam para o mesmo objeto)
```

#### Associação (Membership)

- `in` : "Está em" (Verifica se um item existe dentro de uma sequência)  
- `not in` : "Não está em" (Verifica se um item não existe)

```python
frutas = ["maçã", "banana"]
print("maçã" in frutas)     # True
print("uva" not in frutas)  # True
```

---------
### Sequências de Escape

Usadas dentro de strings para caracteres especiais.

- `\n` : Nova linha
- `\t` : Tabulação (avança o cursor)
- `\r` : Retorno de carro (sobrescreve o início da linha)
- `\\` : Exibe uma barra invertida
- `\'` : Exibe uma aspa simples
- `\"` : Exibe uma aspa dupla

-------------
