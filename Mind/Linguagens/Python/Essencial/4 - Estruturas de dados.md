------------------
### Tupla (Tuple)

Uma coleção **ordenada** e **imutável** (não pode ser alterada após a criação). Permite membros duplicados.
Ideal para dados que não devem mudar, como coordenadas, constantes, etc.

```python
coordenadas = (10, 20)
cores = ("vermelho", "verde", "azul")

# Acesso (igual à lista)
print(coordenadas[1]) # 20

# Utilitários
print(len(cores))
print(cores.count("verde")) # 1
print(cores.index("azul"))  # 2

# Desempacotamento (Unpacking) - Muito comum!
x, y = coordenadas
print(f"X: {x}, Y: {y}") # X: 10, Y: 20
```

-------------
### Conjunto (Set)

Uma coleção **não ordenada**, não indexada e **mutável**. **Não permite membros duplicados**.
Ótimo para remover duplicatas de uma lista ou para operações matemáticas de conjuntos.

```python
numeros = {1, 2, 3, 4, 4, 5} # Resultado: {1, 2, 3, 4, 5}
set_vazio = set() # Obrigatório usar set() para criar um vazio ({} cria um dict)

# Adicionar e Remover
numeros.add(6)
numeros.add(1) # Ignorado, pois 1 já existe
numeros.update([5, 6, 7, 8]) # Adiciona múltiplos itens

numeros.remove(8)     # Remove o 8 (Gera um erro se o item não existir)
numeros.discard(1)    # Remove o 1 (Não gera erro se o item não existir)

# Operações de Conjunto
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b) # União (Union) - {1, 2, 3, 4, 5}
print(a & b) # Interseção (Intersection) - {3}
print(a - b) # Diferença (Difference) - {1, 2}
print(a ^ b) # Diferença Simétrica (Symmetric Difference) - {1, 2, 4, 5}
```

--------------
### Lista (List)
Uma coleção **ordenada** e **mutável**. Permite membros duplicados.

```python
frutas = ["maçã", "banana", "cereja"]
numeros = [1, 5, 2, 8]

# Acesso e Fatiamento
print(frutas[0])      # 'maçã'
print(frutas[1:3])    # ['banana', 'cereja']
print(frutas[-1])     # 'cereja'

# Adicionar
frutas.append("laranja")     # ['maçã', 'banana', 'cereja', 'laranja']
frutas.insert(1, "uva")      # ['maçã', 'uva', 'banana', 'cereja', 'laranja']
frutas.extend(["kiwi", "melão"]) # Adiciona múltiplos itens de outra lista

# Remover
frutas.remove("banana")  # Remove a primeira ocorrência de "banana"
item_removido = frutas.pop(0) # Remove e retorna o item do índice 0 ('maçã')

# Ordenação
numeros.sort()           # Ordena a lista original (in-place)
numeros.sort(reverse=True) # Ordena em ordem decrescente
nova_lista_ordenada = sorted(frutas) # Retorna uma CÓPIA ordenada

# Utilitários
print(len(frutas))
print("uva" in frutas) # True
```

------------
### Dicionário (Dictionary)

Uma coleção de pares **chave-valor**. É mutável. **Nota:** A partir do Python 3.7, dicionários são **ordenados** pela ordem de inserção (antes disso, eram não ordenados).

```python
pessoa = {"nome": "Ana", "idade": 25, "cidade": "São Paulo"}

# Acesso
print(pessoa["nome"]) # 'Ana' (Dá erro se a chave não existir)

# Acesso seguro (Recomendado)
print(pessoa.get("idade"))   # 25
print(pessoa.get("salario")) # None (Não dá erro se não existir)
print(pessoa.get("salario", 0)) # 0 (Define um valor padrão)

# Adicionar ou Atualizar
pessoa["idade"] = 26
pessoa["profissao"] = "Programadora"

# Remover
del pessoa["cidade"]
idade = pessoa.pop("idade") # Remove a chave "idade" e retorna o valor 26

# Iteração
print(pessoa.keys())   # dict_keys(['nome', 'profissao'])
print(pessoa.values()) # dict_values(['Ana', 'Programadora'])
print(pessoa.items())  # dict_items([('nome', 'Ana'), ('profissao', 'Programadora')])
```

--------
### List Comprehension

Uma forma "Pythônica" (concisa e rápida) de criar listas.

```python
# Forma tradicional
quadrados = []
for i in range(10):
    quadrados.append(i * i)

# Com List Comprehension
quadrados_comp = [i * i for i in range(10)]

# Com condicional
pares = [i for i in range(10) if i % 2 == 0]
# [0, 2, 4, 6, 8]
```

---------------------
### Dictionary Comprehension

Uma forma "Pythônica" (concisa e rápida) de criar dicionários.

```python
nomes = ["Ana", "Bruno", "Carla"]
dict_nomes = {nome: len(nome) for nome in nomes}
# {'Ana': 3, 'Bruno': 5, 'Carla': 5}
```

-------------------------
