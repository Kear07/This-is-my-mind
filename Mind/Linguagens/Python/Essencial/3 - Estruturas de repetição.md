---------------------------------------
### While
Executa um bloco de código *enquanto* uma condição for verdadeira.

```python
x = 0
while x < 10:
    print(f"x é = {x}")
    x += 1 # Importante: Incrementar o x para evitar loop infinito
    
```

----------
### While True (com break)

Um loop infinito que depende de uma condição interna (`if`) e do `break` para parar.

```python
x = 0        
while True:
    x += 2
    if x == 100:
        print(f"X é = {x}")
        break # Interrompe o loop
```

-----
### For

Executa um bloco de código para cada item em uma sequência (lista, tupla, string, range, etc).
#### For com range()

A função `range()` gera sequências de números.

- `range(fim)`: Vai de 0 até `fim-1`.
- `range(inicio, fim)`: Vai de `inicio` até `fim-1`.
- `range(inicio, fim, passo)`: Vai de `inicio` até `fim-1`, pulando de `passo` em `passo`.

```python
# Exemplo 1: range(4) -> 0, 1, 2, 3
for x in range(4):  
    print(f"x vale: {x}")

# Exemplo 2: range(1, 11, 2) -> 1, 3, 5, 7, 9
for i in range(1, 11, 2):  # Começa em 1, vai até 10, pulando de 2 em 2
    print(i)
```

#### For com Listas (Iteráveis)

```python
frutas = ["maçã", "banana", "uva"]
for f in frutas:
    print(f"A fruta da vez é {f}")
```

#### For com Dicionários

```python
pessoa = {"nome": "Ana", "idade": 25, "cidade": "São Paulo"}

# Iterando sobre as chaves (padrão)
for chave in pessoa:
    print(chave) # 'nome', 'idade', 'cidade'

# Iterando sobre os pares (chave, valor) - Mais comum
for chave, valor in pessoa.items():
    print(f"{chave}: {valor}")
```

#### For com Strings

```python
palavra = "Kear"
for letra in palavra:
    print(letra) # K, e, a, r
```

#### For com enumerate()

Quando você precisa tanto do **índice** quanto do **valor** durante a iteração.

```python
frutas = ["maçã", "banana", "uva"]
for indice, fruta in enumerate(frutas):
    print(f"Índice {indice} = {fruta}")
# Saída:
# Índice 0 = maçã
# Índice 1 = banana
# Índice 2 = uva
```

------------
### Controle de Loops: break, continue, else

- `break`: Interrompe o loop **imediatamente**. 
- `continue`: Pula a iteração **atual** e vai para a próxima.
- `else`: (Especial do Python) O bloco `else` é executado **apenas se o loop terminar naturalmente** (sem ser interrompido por um `break`).

```python
for num in range(1, 10):
    if num == 3:
        continue # Pula o número 3 e continua o loop

    if num == 7:
        break # Para o loop quando encontrar o 7

    print(num)
else:
    print("Loop terminou sem 'break'!") # Esta linha NÃO será executada

# Saída:
# 1
# 2
# 4
# 5
# 6
```

-------
