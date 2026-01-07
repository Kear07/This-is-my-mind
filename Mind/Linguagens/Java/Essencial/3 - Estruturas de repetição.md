---------------------------
### For

Ideal para quando você sabe o número de iterações (ex: 10 vezes, ou o tamanho de um array).
`for (inicialização; condição; incremento)`

```java
// Contagem de 1 até 5
for (int i = 1; i <= 5; i++) {
    System.out.println("O número atual é: " + i);
}

// Iterando sobre um Array (será visto em Estruturas de Dados)
String[] nomes = {"Kear", "Ana", "Beto"};
for (int i = 0; i < nomes.length; i++) {
    System.out.println(nomes[i]);
}
```

-------
### While

Ideal para quando você não sabe o número de iterações, mas sabe a condição de parada. O bloco pode _nunca_ executar se a condição for falsa de início.

```java
int contador = 0;
while (contador < 5) {
    System.out.println("Contador: " + contador);
    contador++; // Crucial para não criar um loop infinito!
}
```

-----
### Do-While

Similar ao `while`, mas garante que o bloco de código execute **pelo menos uma vez**, pois a condição é verificada _no final_.

```java
// Exemplo clássico: Menu de opções
int opcao;
do {
    System.out.println("1 - Ver Saldo");
    System.out.println("0 - Sair");
    System.out.print("Digite sua opção: ");
    
    // (Aqui viria o código para ler a 'opcao' com o Scanner)
    opcao = 0; // Apenas para o exemplo compilar

} while (opcao != 0); // Repete o menu ENQUANTO a opção não for 0
```

----------
