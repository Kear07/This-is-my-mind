-----------------

O Python lida com aleatoriedade de três formas principais:

1. **`random`**: Uso geral (simulações, jogos, estatística simples). **Não usar para senhas.**
2. **`secrets`**: Criptograficamente seguro (senhas, tokens, auth).
3. **`numpy.random`**: Alta performance, vetores e matrizes (Data Science).

-------
### 1. Uso Geral (`random`)

Biblioteca nativa. Gera números _pseudo-aleatórios_ (se você fixar a semente/seed, o resultado é sempre o mesmo).

```Python
import random

# Fixar a semente (para reproduzir os mesmos resultados em testes)
random.seed(42) 

# Números
f = random.random()          # Float entre 0.0 e 1.0
i = random.randint(1, 10)    # Inteiro entre 1 e 10 (inclui o 10)
r = random.randrange(0, 10, 2) # Inteiro de 0 a 10 (exclui 10), passo 2 (pares)
u = random.uniform(10.5, 20.5) # Float em um intervalo específico

# Sequências
lista = ["A", "B", "C", "D"]

item = random.choice(lista)  # Escolhe 1 item
amostra = random.sample(lista, k=2) # Escolhe k itens únicos (sem reposição)
choices = random.choices(lista, k=5) # Escolhe k itens (com reposição/repetição)

random.shuffle(lista) # Embaralha a lista original (in-place)
print(lista)
```

- `random.randint(a, b)`: Inteiro onde `a <= N <= b`.
- `random.choice(seq)`: Pega um elemento aleatório.
- `random.sample(seq, k)`: Pega elementos únicos (não repete o mesmo índice).
- `random.shuffle(seq)`: Mistura a lista.

-----
### 2. Segurança (`secrets`)

**Sinceridade:** Nunca use `random` para gerar senhas ou tokens de redefinição. É previsível. Use `secrets`.

```Python
import secrets

# Gerar tokens seguros (URLs, Sessões)
token_url = secrets.token_urlsafe(16) # String segura para URL
token_hex = secrets.token_hex(16)     # String hexadecimal (32 chars)
token_bytes = secrets.token_bytes(16) # Bytes brutos

# Escolha segura
senhas = ["1234", "admin", "x9#vP2"]
escolha = secrets.choice(senhas) # Mais seguro que random.choice
```


- `secrets.token_urlsafe(nbytes)`: Gera string aleatória base64 (ótimo para links).
- `secrets.token_hex(nbytes)`: Gera string hexadecimal.

-------
### 3. Alta Performance e Matrizes (`numpy`)

O jeito moderno de usar Numpy (desde a v1.17) é criando um `Generator` (mais rápido e estatisticamente melhor que o antigo `np.random.seed`).

```Python
import numpy as np

# Jeito Moderno (Recomendado)
rng = np.random.default_rng(seed=42)

# Inteiros
arr_int = rng.integers(low=0, high=10, size=(3, 3)) # Matriz 3x3 de 0 a 9

# Floats
arr_float = rng.random(size=5) # 5 floats entre 0.0 e 1.0

# Distribuições Estatísticas
normal = rng.standard_normal(size=1000) # Distribuição Normal (Curva de sino)
poisson = rng.poisson(lam=5, size=100)  # Distribuição de Poisson

# Escolha com pesos (Probabilidades viciadas)
opcoes = ["Ganhar", "Perder"]
probs = [0.1, 0.9] # 10% chance de ganhar, 90% de perder
resultado = rng.choice(opcoes, size=10, p=probs)
```

- `default_rng()`: Cria o gerador.
- `.integers(low, high, size)`: Gera arrays de inteiros.
- `.choice(a, size, replace, p)`: Escolha poderosa com suporte a probabilidades (`p`).

------
### 4. Dados Falsos (`Faker`)

Se você precisa popular um banco de dados para testar sua programação, não fique inventando nomes. Use a lib `Faker` (precisa instalar: `pip install faker`).

```Python
from faker import Faker

fake = Faker('pt_BR') # Gera dados em Português do Brasil!

print(fake.name())       # Ex: "João Silva"
print(fake.address())    # Endereço completo fake
print(fake.cpf())        # CPF válido (no algoritmo), mas fake
print(fake.email())      # e-mail fake
print(fake.text())       # Gera um texto aleatório
print(fake.profile())    # Um dicionário com dados de uma pessoa completa
```

--------
