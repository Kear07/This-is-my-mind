-----------
### Arrays (Vetores)

Arrays em Java têm um **tamanho fixo** definido na sua criação.

```java
// --- Declaração e Inicialização ---

// 1ª Forma: Declarando o tamanho primeiro (cria um array com posições "vazias" [null/0])
String[] nomes = new String[4];

// Atribuindo valores a cada posição (índice)
nomes[0] = "Goku";
nomes[1] = "Vegeta";
nomes[2] = "Gohan";
nomes[3] = "Piccolo";

// 2ª Forma: Declarando já com os valores
double[] notas = { 8.5, 9.0, 7.5, 10.0 };

// --- Acesso e Iteração ---

// Acessando um item pelo índice
System.out.println("O primeiro lutador é: " + nomes[0]); // Saída: Goku

// Pegando o tamanho (propriedade .length)
System.out.println("Total de notas: " + notas.length); // Saída: 4

// Iterando com o loop 'for' clássico
for (int i = 0; i < nomes.length; i++) {
    System.out.println("Posição " + i + ": " + nomes[i]);
}

// Iterando com o loop 'for-each' (Mais simples)
System.out.println("Lista de Notas:");
for (double nota : notas) {
    System.out.println(nota);
}
```

------
### List (Lista)

**Implementação comum: `ArrayList`**

- Coleção **ordenada** (pela ordem de inserção).  
- Permite elementos duplicados.

```java
// Import necessário:
import java.util.ArrayList;
import java.util.List;

// Criação
List<String> frutas = new ArrayList<>();

// Adicionar
frutas.add("Maçã");
frutas.add("Banana");
frutas.add("Maçã"); // Duplicado permitido

// Acessar (por índice)
System.out.println(frutas.get(0)); // Saída: Maçã

// Tamanho
System.out.println(frutas.size()); // Saída: 3

// Iterar
for (String fruta : frutas) {
    System.out.println(fruta);
}

// Remover
frutas.remove("Banana"); // Remove pelo valor
frutas.remove(0);        // Remove pelo índice
```

---------------
### Set (Conjunto)

**Implementação comum: `HashSet`**

- Coleção **não ordenada**.
- **NÃO** permite elementos duplicados.

```java
// Import necessário:
import java.util.HashSet;
import java.util.Set;

// Criação
Set<String> cores = new HashSet<>();

// Adicionar
cores.add("Azul");
cores.add("Verde");
cores.add("Azul"); // Ignorado (duplicado)

// Tamanho
System.out.println(cores.size()); // Saída: 2

// Verificar se contém
System.out.println(cores.contains("Verde")); // Saída: true

// Iterar (ordem não garantida)
for (String cor : cores) {
    System.out.println(cor);
}
```

--------
### Map (Mapa)

**Implementação comum: `HashMap`**

- Coleção de pares **Chave-Valor** (como um Dicionário Python ou Objeto JS).
- Chaves são únicas.

```java
// Import necessário:
import java.util.HashMap;
import java.util.Map;

// Criação
Map<String, Double> notas = new HashMap<>();

// Adicionar/Atualizar (chave, valor)
notas.put("Ana", 9.5);
notas.put("Beto", 8.0);
notas.put("Ana", 10.0); // Atualiza o valor da chave "Ana"

// Acessar (pela chave)
System.out.println(notas.get("Beto")); // Saída: 8.0

// Iterar (pelas chaves)
for (String nome : notas.keySet()) {
    System.out.println(nome + ": " + notas.get(nome));
}

// Iterar (pelos pares chave-valor)
for (Map.Entry<String, Double> entry : notas.entrySet()) {
    System.out.println(entry.getKey() + " = " + entry.getValue());
}
```

------------
