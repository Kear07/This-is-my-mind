-------
### Try / Except (Básico)

O bloco `try` permite testar um bloco de código quanto a erros. O bloco `except` permite lidar com o erro caso ele ocorra, evitando que o programa "quebre" (crash).

``` python
try:
    print(x) # x não foi definido, isso geraria um erro
except:
    print("Ocorreu uma exceção. A variável 'x' não existe.")
```

------
### Capturando Exceções Específicas

É uma boa prática capturar erros específicos em vez de um erro genérico. Você pode ter vários blocos `except`.

``` Python
try:
    dividendo = int(input("Digite um número: "))
    print(10 / dividendo)

except ValueError:
    print("Erro: Você deve digitar um número inteiro!")

except ZeroDivisionError:
    print("Erro: Não é possível dividir por zero!")

except Exception as e:
    # 'Exception' é a classe base genérica. 
    # 'as e' captura a mensagem de erro original para uma variável.
    print(f"Ocorreu um erro inesperado: {e}")
```

-----
### Else e Finally

- `else`: Executado se **nenhum** erro ocorrer no `try`.
    
- `finally`: Executado **sempre**, independentemente de ter ocorrido erro ou não (útil para fechar arquivos ou conexões de banco de dados).

```Python
try:
    print("Tentando abrir o arquivo...")
    # Simulação de código que pode dar erro
    resultado = "Sucesso" 
except:
    print("Algo deu errado.")
else:
    print("Nenhum erro ocorreu!") # Executa apenas se o try funcionar
finally:
    print("O bloco 'try/except' terminou.") # Executa sempre
```

--------
### Raise (Levantando Exceções)

A palavra-chave `raise` é usada para forçar uma exceção quando uma condição específica ocorre. Você define qual tipo de erro quer gerar.

```Python
idade = -5

if idade < 0:
    # Levanta um erro do tipo ValueError e para o programa
    raise ValueError("A idade não pode ser negativa!")
```

-----
### Exceções Personalizadas

Você pode criar seus próprios tipos de exceção criando uma classe que herda de `Exception`. Isso é útil para erros de regras de negócio.

```Python
class SaldoInsuficienteError(Exception):
    pass

def sacar(valor, saldo):
    if valor > saldo:
        raise SaldoInsuficienteError("Você não tem saldo para isso.")
    saldo -= valor
    return saldo

try:
    sacar(100, 50)
except SaldoInsuficienteError as erro:
    print(f"Erro de negócio: {erro}")
```

-----
### Assert (Afirmações)

Usado principalmente para debug e testes (não substitui o `if` na lógica final). Se a condição for `False`, gera um `AssertionError`.

```Python
x = "olá"

# Se a condição for falsa, o programa para e mostra a mensagem
assert x == "olá", "x deveria ser 'olá'"

# Isso vai gerar erro:
# assert x == "tchau", "x deveria ser 'tchau'"
```