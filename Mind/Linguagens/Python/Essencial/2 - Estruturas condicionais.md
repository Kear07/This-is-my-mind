--------------------
### if / else 

Usado para uma decisão simples entre dois caminhos.

```python
Usado para uma decisão simples entre dois caminhos.

```python
idade = 20

if idade >= 18:
    print("Você tem mais de 18 anos, portanto você é um adulto")
else:
    print("Você não tem de 18 anos, portanto você é não é um adulto")
```

-------
### if / elif / else

Usado para testar múltiplas condições em ordem. O Python executa o bloco do _primeiro_ `elif` que for verdadeiro e ignora o resto.

```python
idade = 15

if idade < 4:
    print("Você é um bebê")
elif idade < 12:
    print("você é uma criança")
elif idade < 18:
    print("você é um adolescente") # Este bloco será executado
else:
    print("Você é um adulto")
```

---------
### Operador Ternário (if de uma linha)

Uma forma concisa de escrever um `if/else` simples, geralmente para atribuir um valor.
**Sintaxe:** `valor_se_verdadeiro if condicao else valor_se_falso`

```python
idade = 20
status = "Maior de idade" if idade >= 18 else "Menor de idade"
print(status)
```

----
### Match case 

Usado para comparar um valor contra vários padrões possíveis (similar ao `switch` de outras linguagens).
- `case _:` : É o "wildcard" ou caso padrão (como o `default`), executado se nenhum outro `case` corresponder.

```python
http_status = 404

match http_status:
    case 200:
        print("OK - Sucesso")
    case 404:
        print("Not Found - Não Encontrado")
    case 500:
        print("Internal Server Error - Erro do Servidor")
    case _:
        print("Código de status não reconhecido")
```

-----------------
### Truthiness (O que é True ou False)

Em Python, não são apenas os booleanos que são avaliados em condicionais.

**Valores considerados `False` .

- `False` (o booleano) 
- `None` (ausência de valor)
- `0` (número zero, inteiro ou float)
- `""` (string vazia)
- `[]` (lista vazia)
- `()` (tupla vazia)
- `{}` (dicionário vazio)
- `set()` (conjunto vazio)

Qualquer outro valor é considerado `True` .

```python
minha_lista = []

if minha_lista:
    print("A lista não está vazia.")
else:
    print("A lista está vazia.") # Este bloco será executado
```

-----------------
