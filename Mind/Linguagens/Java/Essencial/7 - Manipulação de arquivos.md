------------

Existem duas "eras" de manipulação de arquivos em Java:
1.  **Java IO (`java.io`)**: A forma clássica, baseada em *Streams* (fluxos de dados).
2.  **Java NIO.2 (`java.nio.file`)**: A forma moderna (Java 7+), baseada em `Path` (caminhos). É mais poderosa e geralmente mais simples para operações comuns.

### Abordagem Moderna (NIO.2 - Recomendada)

Focada nas classes `Path` (o caminho) e `Files` (o que fazer com o caminho).
**Importações necessárias:**
```java
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.Files;
import java.io.IOException; // Quase todas as operações NIO exigem tratamento de exceção
```

--------
#### Criar/Escrever em Arquivos

```java
try {
    // 1. Define o caminho do arquivo
    Path caminho = Paths.get("meu_arquivo.txt");

    // 2. Define o conteúdo
    String conteudo = "Olá, mundo!\nEsta é a segunda linha.";

    // 3. Escreve (cria ou sobrescreve o arquivo)
    Files.writeString(caminho, conteudo);

    // 4. Para adicionar ao final (Append)
    // Files.writeString(caminho, "Mais uma linha\n", StandardOpenOption.APPEND);

} catch (IOException e) {
    System.out.println("Erro ao escrever no arquivo: " + e.getMessage());
}
```

----------
#### Ler Arquivos

```java
try {
    Path caminho = Paths.get("meu_arquivo.txt");

    // Opção 1: Ler o arquivo inteiro para uma String (para arquivos pequenos)
    String conteudo = Files.readString(caminho);
    System.out.println(conteudo);

    // Opção 2: Ler todas as linhas para uma Lista (para arquivos médios)
    List<String> linhas = Files.readAllLines(caminho);
    for (String linha : linhas) {
        System.out.println("Linha lida: " + linha);
    }

} catch (IOException e) {
    System.out.println("Erro ao ler o arquivo: " + e.getMessage());
}
```

---------
#### Verificar, Copiar e Deletar

```java
try {
    Path caminho = Paths.get("meu_arquivo.txt");
    Path caminhoCopia = Paths.get("copia_arquivo.txt");

    // Verificar se existe
    boolean existe = Files.exists(caminho);
    System.out.println("Arquivo existe? " + existe);

    if (existe) {
        // Copiar (sobrescreve se a cópia já existir)
        // Files.copy(caminho, caminhoCopia, StandardCopyOption.REPLACE_EXISTING);
        
        // Deletar
        Files.delete(caminho);
        System.out.println("Arquivo deletado.");
    }

} catch (IOException e) {
    System.out.println("Erro na operação: " + e.getMessage());
}
```

-------
### Abordagem Clássica (IO - Streams)

Esta abordagem é ideal para arquivos muito grandes, pois lê e escreve "sob demanda" (streaming), sem carregar tudo na memória.

**Padrão recomendado: `try-with-resources`** (O `try(...)` fecha os arquivos `writer` e `reader` automaticamente no final, mesmo se um erro ocorrer. Essencial para evitar vazamento de recursos).
#### Escrevendo com `BufferedWriter`

```java
// Importações necessárias:
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

// O 'try-with-resources' gerencia o fechamento
try (FileWriter fileWriter = new FileWriter("log.txt");
     BufferedWriter bufferedWriter = new BufferedWriter(fileWriter)) {

    bufferedWriter.write("Primeira linha do log.");
    bufferedWriter.newLine(); // Adiciona uma quebra de linha
    bufferedWriter.write("Segunda linha do log.");

} catch (IOException e) {
    System.out.println("Erro ao escrever o log: " + e.getMessage());
}
```

-----
#### Lendo com `BufferedReader`

```java
// Importações necessárias:
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

try (FileReader fileReader = new FileReader("log.txt");
     BufferedReader bufferedReader = new BufferedReader(fileReader)) {

    String linha;
    // Lê linha por linha até o final (quando retorna null)
    while ((linha = bufferedReader.readLine()) != null) {
        System.out.println(linha);
    }

} catch (IOException e) {
    System.out.println("Erro ao ler o log: " + e.getMessage());
}
```

-------
