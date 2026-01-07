------------------------
### A Forma Correta: `with open()`

A instrução `with` garante que o arquivo seja fechado automaticamente, mesmo que ocorra um erro. É a forma recomendada de trabalhar com arquivos.

```python
# 'encoding="utf-8"' é crucial para lidar com acentuação e caracteres especiais.
with open('arquivo.txt', 'w', encoding='utf-8') as arquivo:
    arquivo.write("Olá, mundo!\n")
    arquivo.write("Esta é uma nova linha.")
```

-------
### Modos de Abertura

O segundo argumento da `open()` define o modo:

|**Modo**|**Descrição**|
|---|---|
|**`'r'`**|**Leitura (Read)**: Abre para ler. (Padrão). Gera erro se o arquivo não existir.|
|**`'w'`**|**Escrita (Write)**: Abre para escrever. **Apaga** o conteúdo se o arquivo já existir. Cria o arquivo se não existir.|
|**`'a'`**|**Anexar (Append)**: Abre para escrever. **Adiciona** o novo conteúdo ao final do arquivo. Cria se não existir.|
|**`'r+'`**|**Leitura e Escrita**: Abre para ler e escrever. O cursor começa no início.|
|**`'b'`**|**Binário**: Usado em conjunto com outros modos (ex: `'rb'`, `'wb'`) para arquivos que não são texto (imagens, executáveis, etc.).|
|**`'t'`**|**Texto**: Modo padrão. Usado para arquivos de texto.|

---------------
### Métodos de Leitura (Modo 'r')

```python
with open('arquivo.txt', 'r', encoding='utf-8') as arquivo:
    
    # Opção 1: Ler o arquivo inteiro (retorna uma string)
    conteudo_completo = arquivo.read()
    print(conteudo_completo)

    # Nota: Após um .read(), o cursor está no fim.
    # É preciso "rebobinar" para ler de novo:
    arquivo.seek(0) 

    # Opção 2 (Recomendada): Iterar sobre as linhas
    # Mais eficiente em termos de memória para arquivos grandes.
    for linha in arquivo:
        print(linha.strip()) # .strip() remove \n e espaços extras
    
    arquivo.seek(0)
    
    # Opção 3: Ler todas as linhas (retorna uma lista)
    lista_de_linhas = arquivo.readlines()
    print(lista_de_linhas) # ['Olá, mundo!\n', 'Esta é uma nova linha.']
    
    arquivo.seek(0)
    
    # Opção 4: Ler uma única linha (retorna uma string)
    primeira_linha = arquivo.readline()
    print(primeira_linha)
```

-----
### Métodos de Escrita (Modo 'w' ou 'a')

```python
linhas_para_escrever = ["Primeira linha.\n", "Segunda linha.\n", "Terceira linha.\n"]

with open('novo_arquivo.txt', 'w', encoding='utf-8') as arquivo:
    
    # Opção 1: Escrever uma string de cada vez
    arquivo.write("Olá!\n")

    # Opção 2: Escrever todos os itens de uma lista de strings
    arquivo.writelines(linhas_para_escrever)
```

--------
### Módulo `os` (Verificar e Remover)

Para operações no nível do sistema operacional (como verificar se existe ou deletar).

```python
import os
caminho_arquivo = 'novo_arquivo.txt'

if os.path.exists(caminho_arquivo):
    print(f"O arquivo '{caminho_arquivo}' existe.")
    
    # Remover o arquivo
    os.remove(caminho_arquivo)
    print(f"O arquivo '{caminho_arquivo}' foi removido.")
else:
    print(f"O arquivo '{caminho_arquivo}' não existe.")
```

-----
### `pathlib` (A Forma Moderna)

O módulo `pathlib` é a forma moderna e orientada a objetos de lidar com caminhos, substituindo muitas funções do `os`.

```python
from pathlib import Path

# Cria um objeto de caminho
caminho = Path('meu_arquivo.txt')

# Escrever (substitui 'with open...')
caminho.write_text("Conteúdo com pathlib!", encoding='utf-8')

# Ler
conteudo = caminho.read_text(encoding='utf-8')
print(conteudo)

# Verificar se existe
if caminho.exists():
    print("Pathlib diz: Existe!")

# Remover
caminho.unlink()
```

--------
### Manipulando JSON

JSON é um formato de arquivo extremamente comum. O Python tem um módulo nativo para isso.

```python
import json

# Dados do Python (dicionário)
dados = {
    "nome": "Kear",
    "skills": ["Python", "JS", "Java"],
    "ativo": True
}

# 1. Escrever JSON em um arquivo (Serializar)
# 'indent=4' formata o arquivo para ficar legível
with open('dados.json', 'w', encoding='utf-8') as f:
    json.dump(dados, f, indent=4, ensure_ascii=False)

# 2. Ler JSON de um arquivo (Desserializar)
with open('dados.json', 'r', encoding='utf-8') as f:
    dados_lidos = json.load(f)

print(f"Nome lido do JSON: {dados_lidos['nome']}")
```

------
