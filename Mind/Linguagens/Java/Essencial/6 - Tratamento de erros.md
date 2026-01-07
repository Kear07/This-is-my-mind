-------

Exceções (Exceptions) são erros que ocorrem durante a execução do programa e interrompem o fluxo normal.
### Sintaxe de Captura: Try-Catch-Finally

```java
try {
    // Bloco de código que PODE lançar uma exceção
    // Ex: int resultado = 10 / 0; (Lançaria uma ArithmeticException)
    
} catch (Tipo_Excecao variavel_erro) {
    // Bloco para tratar a exceção se ela ocorrer
    // Ex: catch (ArithmeticException e) {
    //     System.out.println("Erro: Divisão por zero não permitida.");
    // }
    
} finally {
    // Bloco (opcional) que será SEMPRE executado,
    // ocorrendo ou não uma exceção.
    // (Ideal para fechar recursos, como 'input.close()')
}
```

---------
### Declarando e Lançando Exceções

#### `throw` (Lançar)

Usado para _criar_ e _lançar_ uma nova exceção manualmente.

```java
public void sacar(double valor) {
    if (valor > this.saldo) {
        // Lança uma exceção (interrompe o método)
        throw new RuntimeException("Saldo insuficiente.");
    }
    this.saldo -= valor;
}
```

----------
#### `throws` (Declarar)

Usado na assinatura de um método para _avisar_ que ele pode lançar uma **Checked Exception**, "empurrando" a obrigação de usar `try-catch` para quem chamar o método.

```java
import java.io.FileNotFoundException;

// Este método AVISA que ele pode lançar FileNotFoundException
public void lerArquivo(String nome) throws FileNotFoundException {
    // ... código que pode lançar a exceção ...
}

// Quem chamar o método agora é OBRIGADO a tratar:
public void meuMetodo() {
    try {
        lerArquivo("meu-arquivo.txt");
    } catch (FileNotFoundException e) {
        System.out.println("Arquivo não encontrado!");
    }
}
```

--------
### Unchecked Exceptions (Exceções Não Verificadas)

- **O que são:** Erros de lógica do programador (`RuntimeException`).
- **Exemplos:** `NullPointerException` (variável nula), `ArrayIndexOutOfBoundsException` (índice de array errado), `ArithmeticException` (divisão por zero).
- **Regra:** O Java **não** te obriga a usar `try-catch` ou `throws` para elas.

------
### Checked Exceptions (Exceções Verificadas)

- **O que são:** Erros externos que o programa não pode prever (ex: o arquivo não existe, a rede caiu).
- **Exemplos:** `IOException`, `FileNotFoundException`, `SQLException`.
- **Regra:** O Java **obriga** você a tratá-las (seja com `try-catch` ou `throws`).

-----
### Lista de Referência de Exceções

- `NullPointerException`: Tentativa de usar uma variável com valor `null`. 
- `ArrayIndexOutOfBoundsException`: Tentativa de acessar um índice de array que não existe.
- `FileNotFoundException`: Tentativa de abrir um arquivo que não existe.
- `IOException`: Exceção genérica para qualquer falha de Entrada/Saída (I/O).
- `SQLException`: Indica um erro durante uma operação com um banco de dados.
- `RuntimeException`: Classe base para a maioria dos erros de programação (Unchecked).
- `AlreadyBoundException`: Tentativa de salvar um arquivo com um nome que já existe no diretório.
- `BadBinaryOpValueExpException`: Argumentos de uma operação binária (ex: "maior que") são incompatíveis.
- `BadLocationException`: Tentativa de acessar uma posição inválida em um documento de texto.
- `BadStringOperationException`: Tentativa de aplicar uma operação de string em um valor que não é uma string.
- `CloneNotSupportedException`: Chamada do método `clone()` em um objeto que não implementa a interface.
- `DataFormatException`: Formato de dados incorreto (comum em compressão/descompressão).
- `FontFormatException`: Tentativa de criar uma fonte de um arquivo corrompido.
- `GeneralSecurityException`: Falhas em criptografia ou verificação de assinaturas.
- `IllegalClassFormatException`: Mudança de formato bytecode de uma variável inválida.
- `IntrospectionException`: Erro durante o processo de introspecção de uma classe.
- `JMException`: Exceção genérica para problemas no JMX.
- `KeySelectorException`: Falha ao tentar encontrar a chave de APIs.
- `LambdaConversionException`: Problema ao converter uma expressão lambda.
- `MimeTypeParseException`: String de MIME Type (como "text/html") malformada.
- `ParseException`: Erro ao converter o tipo de um objeto/variável.
- `NamingException`: Erro genérico durante uma operação de busca em um diretório.
- `NotBoundException`: Tentativa de procurar um recurso por um nome não registrado.
- `PropertyVetoException`: Alteração em uma propriedade de um objeto é vetada (rejeitada).
- `ReflectiveOperationException`: Captura exceções relacionadas à Reflexão (Reflection).
- `RefreshFailedException`: Falha ao renovar credenciais de segurança.
- `ScriptException`: Erro durante a execution de um script.
- `StringConcatException`: Erro durante o processo otimizado de concatenação de strings.
- `TimeoutException`: Operação excede o tempo limite.
- `TransformerException`: Erro grave durante a transformação de um documento XML.
- `UnmodifiableClassException`: Tentativa de modificar uma classe que não permite modificações (agente Java).
- `UnsupportedFlavorException`: Solicitação de dados em um formato específico, mas os dados estão em outros formatos.
- `XMLParseException`: Erro básico durante a análise de um documento XML.
- `SAXException`: Erro avançado durante a análise de um documento XML.
- `XPathException`: Erro em uma expressão XPath.
- `VMStartException`: Falha ao iniciar uma JVM alvo.
- `XAException`: Erro sério em um gerenciador de recursos.

----------
