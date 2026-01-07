------------------

POO é um paradigma para estruturar o código em "objetos", que agrupam dados (atributos) e comportamentos (métodos).
### Classe, Construtor (`__init__`) e `self`

* `class`: Palavra-chave para criar uma classe.
* `self`: Refere-se à instância (o objeto específico) que está sendo criada ou usada. É o `this` do JavaScript.
* `__init__`: O "construtor" (Método Mágico/Dunder). É chamado automaticamente quando você cria um objeto da classe (ex: `p1 = Pessoa()`).

```python
class Pessoa:
    # 1. O construtor inicializa os atributos (dados)
    def __init__(self, nome, idade):
        self.nome = nome
        self.idade = idade
        print(f"Objeto Pessoa para '{self.nome}' criado!")

    # 2. Um método (comportamento)
    def saudar(self):
        print(f"Olá, meu nome é {self.nome} e eu tenho {self.idade} anos.")

# 3. "Instanciar" a classe (criar os objetos)
p1 = Pessoa("Ana", 30)
p2 = Pessoa("João", 25)

# 4. Chamar os métodos
p1.saudar()
p2.saudar()
```

----------------------
### Encapsulamento (Níveis de Acesso)

Define a visibilidade dos atributos e métodos. Python usa convenções:

- `self.publico`: Acesso livre de qualquer lugar. 
- `self._protegido`: **Convenção.** "Sugere" que não deve ser acessado de fora da classe ou de suas filhas.
- `self.__privado`: **Realmente privado.** O Python renomeia o atributo internamente (processo "name mangling") para evitar acesso externo acidental.

```python
class Cofre:
    def __init__(self, senha):
        self.publico = "Qualquer um pode ver"
        self._protegido = "Estou protegido por convenção"
        self.__privado = senha # Atributo privado

    def get_senha(self):
        # Métodos de dentro da classe podem acessar o privado
        return self.__privado

meu_cofre = Cofre("1234")
print(meu_cofre.publico)     # Acesso OK
print(meu_cofre._protegido)  # Acesso OK (mas "feio")
# print(meu_cofre.__privado) # ERRO! Não pode ser acessado
print(meu_cofre.get_senha()) # OK
```

---------------
### Herança

Permite que uma classe (filha) herde atributos e métodos de outra classe (mãe).
- `super()`: Usado para chamar métodos da classe mãe (principalmente o `__init__`).

```python
# 'Pessoa' é a classe mãe
class Funcionario(Pessoa):
    # O construtor da filha recebe os dados da mãe + os seus
    def __init__(self, nome, idade, salario):
        # 1. Chama o construtor da classe mãe
        super().__init__(nome, idade)
        
        # 2. Adiciona os novos atributos
        self.salario = salario

    # Novo método
    def exibir_salario(self):
        print(f"O salário de {self.nome} é R${self.salario}")

func1 = Funcionario("Carlos", 45, 5000)
func1.saudar() # Método herdado de Pessoa
func1.exibir_salario() # Método da própria classe Funcionario
```

------
### Polimorfismo

"Muitas formas". É a ideia de que objetos de classes diferentes podem ter métodos com o _mesmo nome_, mas com _comportamentos diferentes_.

```python
class Passaro:
    def mover(self):
        print("Voando")

class Peixe:
    def mover(self):
        print("Nadando")

# O código que usa os objetos não precisa saber o tipo exato,
# apenas que eles têm um método .mover()
for animal in [Passaro(), Peixe()]:
    animal.mover()

# Saída:
# Voando
# Nadando
```

-----------
### Abstração (Classes Abstratas)

Força que classes filhas implementem métodos específicos. Útil para "contratos" e Polimorfismo.

- `ABC`: (Abstract Base Class) Define a classe como abstrata.
- `@abstractmethod`: Marca um método que _deve_ ser implementado pelas classes filhas.

```python
from abc import ABC, abstractmethod

class Animal(ABC):   # Classe Abstrata (não pode ser instanciada)
    @abstractmethod
    def mover(self):
        pass # Não faz nada aqui

class Passaro(Animal):
    # A classe filha é OBRIGADA a implementar o método 'mover'
    def mover(self):
        print("Voando")

class Peixe(Animal):
    # A classe filha é OBRIGADA a implementar o método 'mover'
    def mover(self):
        print("Nadando")

# p = Animal() # ERRO! Não se pode instanciar classe abstrata
p = Passaro()
p.mover()
```

--------------
### Decoradores de POO

Modificadores que alteram o comportamento de métodos.

- `@property`: Transforma um método em um atributo (chamado sem `()`). Usado para "getters".
- `@[nome].setter`: Define um "setter" para o `@property`.
- `@classmethod`: Um método que atua sobre a **Classe** (usa `cls`), não sobre a instância (`self`).
- `@staticmethod`: Uma função auxiliar que fica "dentro" da classe, mas não usa `self` nem `cls`.

```python
class Circulo:
    def __init__(self, raio):
        self._raio = raio # Atributo "protegido"

    # Getter: Permite ler 'c.raio'
    @property
    def raio(self):
        print("Chamando o @property (getter)")
        return self._raio

    # Setter: Permite fazer 'c.raio = 10'
    @raio.setter
    def raio(self, novo_raio):
        if novo_raio > 0:
            self._raio = novo_raio
        else:
            print("O raio deve ser positivo!")
    
    @classmethod
    def criar_circulo_padrao(cls):
        # 'cls' é a própria classe 'Circulo'
        return cls(raio=1) # Chama o __init__ com raio 1

    @staticmethod
    def pi():
        return 3.14159

# Testando @property e @setter
c = Circulo(5)
print(c.raio) # Chama o @property
c.raio = 10   # Chama o @setter
c.raio = -2   # Chama o @setter (e falha)

# Testando @classmethod
c_padrao = Circulo.criar_circulo_padrao()
print(c_padrao.raio) # 1

# Testando @staticmethod
print(Circulo.pi()) # 3.14159
```

------
### Métodos Mágicos (Dunder Methods)

Métodos com duplo underscore (`__`) que permitem que seus objetos usem operadores nativos do Python (como `+`, `==`, `len()`, `print()`).

| **Métodos**             | **Chamado quando**         | **Exemplo de uso**                    | **Quando usar**                                 |
| ----------------------- | -------------------------- | ------------------------------------- | ----------------------------------------------- |
| `__init__`              | Objeto é criado            | `p = Pessoa("Ana", 30)`               | Sempre que quiser inicializar atributos         |
| `__str__`               | `print(obj)` ou `str(obj)` | `print(p)` → `"Nome: Ana, Idade: 30"` | Mostrar texto legível para humanos              |
| `__repr__`              | `repr(obj)`                | `Pessoa(nome='Ana', idade=30)`        | Mostrar formato útil para debug/desenvolvedores |
| `__eq__`                | `obj1 == obj2`             | `p1 == p2`                            | Comparar conteúdo de objetos                    |
| `__ne__`                | `obj1 != obj2`             | `p1 != p2`                            | Comparar desigualdade                           |
| `__le__`                | `obj1 <= obj2`             | `p1 <= p2`                            | Ordenações personalizadas                       |
| `__gt__`                | `obj1 > obj2`              | `p1 > p2`                             | Comparações personalizadas                      |
| `__ge__`                | `obj1 >= obj2`             | `p1 >= p2`                            | Comparações personalizadas                      |
| `__add__`               | `obj1 + obj2`              | `p1 + p2`                             | Quando quer definir uma “adição” própria        |
| `__sub__`               | `obj1 - obj2`              | `p1 - p2`                             | Definir subtração                               |
| `__mul__`               | `obj1 * obj2`              | `p1 * p2`                             | Definir multiplicação                           |
| `__truediv__`           | `obj1 / obj2`              | `p1 / p2`                             | Definir divisão                                 |
| `__len__`               | `len(obj)`                 | `len(turma)`                          | Definir tamanho do objeto                       |
| `__getitem__`           | `obj[idx]`                 | `obj[0]`                              | Acesso como lista/dicionário                    |
| `__setitem__`           | `obj[idx] = valor`         | `obj[0] = "João"`                     | Modificar item                                  |
| `__delitem__`           | `del obj[idx]`             | `del obj[0]`                          | Remover item                                    |
| `__contains__`          | `valor in obj`             | `"Ana" in turma`                      | Busca rápida                                    |
| `__call__`              | `obj()`                    | `p()`                                 | Tornar objeto “função”                          |
| `__enter__ / __exit__`  | `with obj:`                | `with Pessoa("Ana", 30):`             | Contexto (abrir/fechar recursos)                |
| `__iter__` / `__next__` | `for x in obj`             | `for aluno in turma`                  | Iterar sobre o objeto                           |
Exemplos Práticos (Dunders)

```python
class Pessoa:
    def __init__(self, nome, idade):
        self.nome = nome
        self.idade = idade

    def __str__(self):
        # Chamado pelo print()
        return f"{self.nome} ({self.idade} anos)"

    def __repr__(self):
        # Chamado em modo debug
        return f"Pessoa(nome={self.nome!r}, idade={self.idade})"

    def __eq__(self, other):
        # Chamado pelo '=='
        return self.nome == other.nome and self.idade == other.idade

    def __add__(self, other):
        # Chamado pelo '+'
        return self.idade + other.idade

    def __len__(self):
        # Chamado pelo len()
        return len(self.nome)

    def __call__(self):
        # Chamado pelo obj()
        print(f"Olá, meu nome é {self.nome}!")

# Testando
p1 = Pessoa("Ana", 30)
p2 = Pessoa("João", 25)
p3 = Pessoa("Ana", 30)

print(p1)           # __str__ -> Ana (30 anos)
print(repr(p1))     # __repr__ -> Pessoa(nome='Ana', idade=30)
print(p1 == p3)     # __eq__ -> True
print(p1 == p2)     # __eq__ -> False
print(p1 + p2)      # __add__ -> 55 (soma das idades)
print(len(p1))      # __len__ -> 3 (tamanho do nome "Ana")
p1()                # __call__ -> Olá, meu nome é Ana!
```

---------
### Dataclasses

Um decorador que escreve automaticamente o `__init__`, `__repr__`, `__eq__` e outros métodos para você, economizando código.

```python
from dataclasses import dataclass

@dataclass
class PessoaData:
    nome: str
    idade: int
    profissao: str = "N/A" # Valor padrão

# Isso já cria __init__, __repr__, __eq__ automaticamente.
p_data1 = PessoaData("Maria", 40)
p_data2 = PessoaData("Maria", 40)

print(p_data1)      # __repr__ -> PessoaData(nome='Maria', idade=40, profissao='N/A')
print(p_data1 == p_data2) # __eq__ -> True
```

------------
