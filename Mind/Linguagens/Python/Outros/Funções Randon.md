------
### Funções de load

```python
def loading(tempo):
    """
    Função que simula um carregamento na tela.
    Parâmetros:
    tempo (int): Tempo em segundos para o carregamento.
    """
    import os
    import time
    
    pontos = ["  ", ".", "..", "..."]
    
    for i in range(tempo):  
        print(f"\rLoading{pontos[i % len(pontos)]} ", end="", flush=True)
        time.sleep(0.3)  
       
    # Limpa o terminal após o carregamento
    os.system('cls')
```
------
```python
# Função de loading
def loading2(tempo):
    """
    Função que simula um carregamento na tela com uma barra de progresso.
    Parâmetros:
    tempo (int): Tempo em segundos para o carregamento.
    """

    for i in range(tempo):
        percent = int((i + 1) / tempo * 100)
        bar = '█' * (percent // 2) + '-' * (50 - percent // 2)
        print(f"\r|{bar}| {percent}%", end="", flush=True)
        time.sleep(tempo*0.3)
    # Limpa o terminal
    os.system('cls')
```
----
```python
def loading3(tempo):
    """

    Função que simula um carregamento na tela com uma barra de progresso e tempo estimado.

    Parâmetros:

    tempo (int): Tempo em segundos para o carregamento. Divido por 4

    20 = 5 segundos de carregamento.

    100 = 25 segundos de carregamento.

    """

    import os
    import time

    for i in range(tempo):

        percent = int((i + 1) / tempo * 100)

        bar = '›' * (percent // 2) + '-' * (50 - percent // 2)

        estimated_time = (tempo - i - 1) * 0.3

        print(f"\rⅠ{bar}Ⅰ {percent}% | Estimativa: {estimated_time:.1f}s", end="", flush=True)

        time.sleep(0.3)

    # Limpa o terminal após o carregamento
    os.system('cls')
```
----------
```python
def loading4(tempo):
    """

    Funcão com animação de carregamento com | / - \ |.
    """
    
    import os
    import time

    # animacao = ['|', '/', '—', '\\']

    animacao = ['⌈⌋', '⌊⌉']

  

    for i in range(tempo):

        print(f"\rLoading {animacao[i % len(animacao)]}", end="", flush=True)

        time.sleep(0.2)


    # Limpa o terminal após o carregamento
    os.system('cls')
```
-------
### Gerador de números

```python
def random(quantidade):

    import os
    import time 
    import random as rd

    numbers = rd.sample(range(0, 101), quantidade)

    for i in range(quantidade):

        print(f"\rGenerating {numbers[i]}", end="", flush=True)

        time.sleep(0.2)

    os.system('cls')
    print(numbers)
```

-------
### Gerador de senhas

```python
def gerar_senha(tamanho):

    import random as rd
    import string
    import time

    caracteres = string.ascii_letters + string.digits + "!@#$%&*"

    print("🔐 Gerando senha:", end="", flush=True)

    for _ in range(tamanho):

        print(" ×", end="", flush=True)

        time.sleep(0.1)

    senha = ''.join(rd.choices(caracteres, k=tamanho))

    print(f"\nSenha gerada: {senha}")
```

----

