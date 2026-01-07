------------

Strings em Python são **imutáveis**, o que significa que os métodos não alteram a string original, eles sempre **retornam uma nova string** com a modificação.
### Fatiamento (Slicing)

(Não é um método, é uma sintaxe)

```python
texto = "Python"
print(texto[2:6])   # Saída: "thon"
print(texto[:2])    # Saída: "Py"
print(texto[:-2])    # Saída: "Pyth"
print(texto[3:])    # Saída: "hon"
print(texto[::2])   # Saída: "Pto" (passo 2)
print(texto[::-1])  # Saída: "nohtyP" (Inverte a string)
```

- `string[inicio:fim]`
- `string[inicio:fim:passo]`

------
### Formatação (Saída)

```python
nome = "Kear"
# f-string (Recomendado)
print(f"Olá, {nome}!")

# .format() (Antigo)
print("Olá, {}".format(nome))
```

--------------
### Verificações (Retornam True/False)

```python
texto = "  "
num = "123"
alfa = "Python"

print(texto.isspace())   # True
print(num.isnumeric())   # True
print(alfa.isalpha())    # True
print(alfa.isupper())    # False
print(alfa.islower())    # False
print("Curso Python".istitle()) # True
```

- `.isspace()`: Retorna `True` se todos os caracteres são espaços.
- `.isnumeric()`: Retorna `True` se todos são números.
- `.isalpha()`: Retorna `True` se todos são letras.
- `.isalnum()`: Retorna `True` se são letras OU números.
- `.isupper()`: Retorna `True` se todos estão em maiúsculas.
- `.islower()`: Retorna `True` se todos estão em minúsculas.
- `.istitle()`: Retorna `True` se cada palavra começa com maiúscula.
------
### Transformação (Retornam nova string)

```python
texto = "bem-vindo ao Python"
print(texto.upper())       # BEM-VINDO AO PYTHON
print(texto.lower())       # bem-vindo ao python
print(texto.capitalize())  # Bem-vindo ao python
print(texto.title())       # Bem-Vindo Ao Python
print("PyThOn".swapcase()) # pYtHoN
```

- `.upper()`: Converte toda a string para maiúsculas.
- `.lower()`: Converte toda a string para minúsculas.
- `.capitalize()`: Converte apenas o primeiro caractere da string para maiúsculo.
- `.title()`: Converte o primeiro caractere de cada palavra para maiúsculo.
- `.swapcase()`: Inverte o caso (maiúsculas viram minúsculas e vice-versa).
 
----
### Limpeza (Espaços e Caracteres)

```python
email = "  kear@email.com  "
print(email.strip())  # "kear@email.com"
print(email.lstrip()) # "kear@email.com  "
print(email.rstrip()) # "  kear@email.com"
```

- `.strip()`: Remove espaços (ou caracteres especificados) do início E do fim.
- `.lstrip()`: Remove espaços (ou caracteres especificados) do início (esquerda).
- `.rstrip()`: Remove espaços (ou caracteres especificados) do fim (direita).

-------
### Busca & Substituição

```python
frase = "Eu amo Python, Python é incrível"

print(frase.find("Python"))     # 7 (retorna o índice da primeira)
print(frase.find("Java"))       # -1 (não encontrou)
# .index() faz o mesmo, mas dá erro se não encontrar.

print(frase.replace("Python", "JS")) # "Eu amo JS, JS é incrível"
print(frase.count("Python"))         # 2
print(frase.startswith("Eu amo"))    # True
print(frase.endswith("incrível"))  # True
```

- `.find(valor)`: Retorna o índice da primeira ocorrência de `valor`. Retorna -1 se não encontrar.
- `.replace(velho, novo)`: Substitui todas as ocorrências de `velho` por `novo`.
- `.count(valor)`: Conta quantas vezes `valor` aparece.
- `.startswith(valor)`: Retorna `True` se a string começa com `valor`.
- `.endswith(valor)`: Retorna `True` se a string termina com `valor`.

--------
### Divisão & Junção

```python
# Divisão (str -> list)
dados = "Kear;25;Programador"
lista_dados = dados.split(";")
print(lista_dados) # ['Kear', '25', 'Programador']

# Junção (list -> str)
separador = " - "
texto_junto = separador.join(lista_dados)
print(texto_junto) # "Kear - 25 - Programador"
```

- `.split(sep)`: Divide a string em uma `list`, usando `sep` como separador.
- `sep.join(lista)`: Junta os itens de uma `list` em uma `str`, usando `sep` como cola.

------
### Alinhamento e Preenchimento

```python
codigo = "42"
print(codigo.zfill(5))      # "00042"
print("Olá".ljust(10, "."))  # "Olá......."
print("Olá".rjust(10, "."))  # ".......Olá"
print("Olá".center(10, "=")) # "===Olá==="
```

- `.zfill(n)`: Preenche com zeros à esquerda até o tamanho `n`.
- `.ljust(n, char)`: Justifica à esquerda (preenche à direita) com `char` até o tamanho `n`.
- `.rjust(n, char)`: Justifica à direita (preenche à esquerda) com `char` até o tamanho `n`.
- `.center(n, char)`: Centraliza, preenchendo com `char` em ambos os lados até o tamanho `n`.

---------
