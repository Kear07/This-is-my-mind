-------------------------
### Tipos de Dados Primitivos

| Tipo          | Descrição                                                                 | Exemplo                         |
| :------------ | :------------------------------------------------------------------------ | :------------------------------ |
| **`String`**  | Tipo texto. (Nota: `String` é uma Classe, não um primitivo, mas é básico) | `String nome = "Kear";`         |
| **`int`**     | Tipo inteiro numérico. (32 bits)                                          | `int idade = 25;`               |
| **`double`**  | Tipo decimal numérico (ponto flutuante). (64 bits)                        | `double salario = 1250.75;`     |
| **`boolean`** | Tipo verdadeiro ou falso.                                                 | `boolean ativo = true;`         |
| **`char`**    | Um único caractere (deve usar aspas simples).                             | `char letra = 'A';`             |
| **`long`**    | Inteiro longo (64 bits).                                                  | `long populacao = 7000000000L;` |
| **`float`**   | Decimal (precisão menor que double).                                      | `float altura = 1.75f;`         |

---
### Operadores

#### Operadores Matemáticos
* `+`: Soma
* `-`: Subtração
* `*`: Multiplicação
* `/`: Divisão
    * `10 / 3` = `3` (Divisão inteira, pois ambos são `int`)
    * `10.0 / 3.0` = `3.33...` (Divisão `double`, pois os números são `double`)
* `%`: Módulo (Resto da divisão)

----
#### Operadores de Comparação
* `==`: Igual a
* `!=`: Diferente de
* `<`: Menor que
* `>`: Maior que
* `<=`: Menor ou igual a
* `>=`: Maior ou igual a

-------
#### Operadores Lógicos
* `&&`: Operador "E" (AND)
* `||`: Operador "OU" (OR)
* `!`: Operador de negação "NÃO" (NOT)

--------------------
#### Operadores de Atribuição
* `=`: Define/Atribui valor.
* `+=`: Adiciona e atribui (`x += 1` é o mesmo que `x = x + 1`)
* `-=`: Subtrai e atribui.
* `*=`: Multiplica e atribui.
* `/=`: Divide e atribui.

---
### Entrada de Dados (Scanner)

Para ler dados do terminal, usamos a classe `Scanner`.

```java
// 1. Importa a classe (fora da classe principal, no topo do arquivo)
import java.util.Scanner;

// 2. Cria o objeto "leitor" (dentro do método main)
Scanner input = new Scanner(System.in);

// 3. Pede e lê os dados
System.out.print("Digite seu nome: ");
String nome = input.nextLine(); // Lê a linha inteira (texto)

System.out.print("Digite sua idade: ");
int idade = input.nextInt(); // Lê o próximo número inteiro

System.out.print("Digite sua altura: ");
double altura = input.nextDouble(); // Lê o próximo número double

// 4. Fecha o input (ao final do uso)
input.close();
```

-------
### Saída de Dados (Print)

```java
// Exibe algo e pula uma linha no final.
System.out.println("Olá, mundo!");

// Exibe algo sem pular a linha no final.
System.out.print("Meu nome é ");
System.out.print("Kear.");
// Saída: Meu nome é Kear.

// Exibe texto formatado (similar ao C ou Python).
System.out.printf("Me chamo %s e tenho %d anos.%n", nome, idade);
```

Códigos `printf`:

- `%s`: Para textos (String)
- `%d`: Para números inteiros (int, long)
- `%f`: Para números com casas decimais (double, float)
- `%c`: Para um único caractere (char)
- `%b`: Para valores booleanos (true/false)    
- `%n`: Pula uma linha (preferível ao `\n` no `printf`)

-------
### Sequências de Escape

Usadas dentro de Strings (`""`) para caracteres especiais.

- `\n`: Nova Linha (pula uma linha) 
- `\t`: Tabulação (adiciona um espaço de tabulação)
- `\r`: Retorno de Carro
- `\\`: Exibe uma barra invertida (`\`)
- `\"`: Exibe aspas duplas (`"`)

-------------
