---------

Executam blocos de código com base em condições booleanas (`true` ou `false`).
### IF / ELSE IF / ELSE

```java
int idade = 20;

if (idade < 18) {
    System.out.println("Menor de idade.");
} else if (idade >= 18 && idade < 65) {
    System.out.println("Adulto.");
} else {
    System.out.println("Idoso.");
}
```

---------------
### Switch Case

Uma alternativa ao `if/else if/else` para testar uma variável contra múltiplos valores exatos.
**Importante:** O `break` é essencial para parar a execução. Sem ele, o Java continuará executando os `case` abaixo ("fall-through").

```java
int diaDaSemana = 1; // 1 = Domingo, 2 = Segunda...

switch (diaDaSemana) {
    case 1:
        System.out.println("Domingo");
        break;
    case 2:
        System.out.println("Segunda-feira");
        break;
    case 7:
        System.out.println("Sábado");
        break;
    default:
        // Código executado se nenhum dos casos acima for correspondido
        System.out.println("Dia inválido");
}
```

----
### Dica de Ouro: `==` vs `.equals()` (Strings)

Para comparar Strings em Java, **NUNCA** use `==`. Use sempre o método `.equals()`.

- `if (nome == "Ana")` ❌ **ERRADO** (Compara a posição na memória, não o conteúdo).  
- `if (nome.equals("Ana"))` ✅ **CORRETO** (Compara o conteúdo do texto).
- `if (nome.equalsIgnoreCase("ana"))` ✅ (Compara o conteúdo, ignorando maiúsculas/minúsculas).