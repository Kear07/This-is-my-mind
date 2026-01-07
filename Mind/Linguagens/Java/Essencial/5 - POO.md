------------
### Classes e Objetos

* **Classe:** O "molde". Define o que um objeto terá. (Ex: `Funcionario.java`).
* **Objeto:** A instância da classe. É o "João" ou a "Maria", criados a partir do molde `Funcionario`.

```java
// Arquivo: Funcionario.java
public class Funcionario {
    // Atributos (Características)
    String nome;
    String cargo;
    double salario;

    // Métodos (Ações)
    void trabalhar() {
        System.out.println(nome + " está trabalhando...");
    }
}

// Arquivo: Main.java
public class Main {
    public static void main(String[] args) {
        // new Funcionario() -> Cria o objeto na memória (instanciação)
        Funcionario func1 = new Funcionario();

        // Definindo os valores dos atributos
        func1.nome = "João Silva";
        func1.cargo = "Desenvolvedor";
        func1.salario = 5000.0;

        // Usando o objeto
        System.out.println("Funcionário: " + func1.nome); // Saída: Funcionário: João Silva
        func1.trabalhar(); // Saída: João Silva está trabalhando...
    }
}
```

---------
### Construtores

Um método especial que é chamado automaticamente quando um `new` é usado. Serve para obrigar a definição de atributos essenciais na criação.

```java
// Arquivo: Funcionario.java
public class Funcionario {
    String nome;
    String cargo;
    double salario;

    // Construtor: Obriga a definir nome e cargo na criação
    public Funcionario(String nome, String cargo) {
        this.nome = nome; // 'this' diferencia o atributo da classe do parâmetro
        this.cargo = cargo;
        this.salario = 1500.0; // Salário base padrão
    }

    void trabalhar() {
        System.out.println(nome + " está trabalhando...");
    }
}

// Arquivo: Main.java
public class Main {
    public static void main(String[] args) {
        // Agora, os dados são passados na criação do objeto
        Funcionario func1 = new Funcionario("Maria Souza", "Analista de RH");
        
        System.out.println(func1.nome + " - " + func1.cargo); // Saída: Maria Souza - Analista de RH
    }
}
```

-------
### Encapsulamento (private, getters, setters)

Protege os atributos da classe contra acesso direto (`private`). O controle é feito por métodos públicos (`get` para ler, `set` para alterar).

```java
// Arquivo: Funcionario.java
public class Funcionario {
    private String nome;
    private String cargo;
    private double salario;

    public Funcionario(String nome, String cargo) {
        this.nome = nome;
        this.cargo = cargo;
        this.salario = 1500.0;
    }

    // Getter para nome (Permite ler)
    public String getNome() {
        return this.nome;
    }

    // Setter para salário (Permite alterar com uma regra)
    public void setSalario(double novoSalario) {
        if (novoSalario > this.salario) {
            this.salario = novoSalario;
        } else {
            System.out.println("O novo salário não pode ser menor que o atual.");
        }
    }
    
    // Getter para salário
    public double getSalario() {
        return this.salario;
    }
}

// Arquivo: Main.java
public class Main {
    public static void main(String[] args) {
        Funcionario func1 = new Funcionario("Carlos Lima", "Gerente");

        // func1.salario = 1000; // ❌ ERRO! 'salario' é private.

        // Jeito correto de interagir
        func1.setSalario(8000.0);
        System.out.println("Salário do " + func1.getNome() + ": R$" + func1.getSalario()); // Saída: R$8000.0
    }
}
```

----
### Herança (extends, super)

Permite que uma classe (filha) herde atributos e métodos de outra classe (mãe).

```java
// Arquivo: Gerente.java
// Gerente "é um" Funcionario, então ele herda (extends)
public class Gerente extends Funcionario {
    
    private int numeroDeSubordinados;

    // Construtor do Gerente
    public Gerente(String nome, String cargo, int numeroDeSubordinados) {
        // super() -> Chama o construtor da classe mãe (Funcionario)
        // Deve ser a primeira linha do construtor filho
        super(nome, cargo); 
        this.numeroDeSubordinados = numeroDeSubordinados;
    }

    // Método específico do Gerente
    public void aprovarDespesa() {
        System.out.println("Despesa aprovada pelo gerente " + getNome()); // getNome() foi herdado
    }
}

// Arquivo: Main.java
public class Main {
    public static void main(String[] args) {
        Gerente gerente1 = new Gerente("Ana Paula", "Gerente de Projetos", 5);
        
        // Métodos e atributos (acessados via getters) herdados de Funcionario
        System.out.println("Salário: " + gerente1.getSalario());
        
        // Método próprio do Gerente
        gerente1.aprovarDespesa(); // Saída: Despesa aprovada pelo gerente Ana Paula
    }
}
```

--------
### Polimorfismo (@Override)

"Muitas formas". É a capacidade de um método com o mesmo nome se comportar de formas diferentes em classes diferentes (mãe e filhas).

```java
// Arquivo: Funcionario.java
public class Funcionario {
    // ... (atributos e construtor anteriores) ...
    
    // Método de bônus padrão
    public double calcularBonus() {
        return this.getSalario() * 0.10; // Bônus de 10% para funcionários
    }
    // ... (getters/setters) ...
}


// Arquivo: Gerente.java
public class Gerente extends Funcionario {
    // ... (atributos e construtor anteriores) ...

    @Override // Sobrescrevendo o método para ter um comportamento diferente
    public double calcularBonus() {
        return this.getSalario() * 0.25; // Bônus de 25% para gerentes
    }
}

// Arquivo: Main.java
public class Main {
    public static void main(String[] args) {
        Funcionario func = new Funcionario("João Silva", "Desenvolvedor");
        func.setSalario(5000);

        Gerente gerente = new Gerente("Ana Paula", "Gerente de Projetos", 5);
        gerente.setSalario(10000);

        // O mesmo método se comporta de formas diferentes
        System.out.println("Bônus do Funcionário: " + func.calcularBonus());   // Saída: 500.0
        System.out.println("Bônus do Gerente: " + gerente.calcularBonus()); // Saída: 2500.0
    }
}
```

---------------
### Abstração (Classes Abstratas)

Transforma uma classe em um "contrato" ou "modelo" que não pode ser instanciado (`new`), apenas herdado. Pode forçar as classes filhas a implementarem métodos.

```java
// abstract -> Agora é um modelo, não pode ser criado um "new Funcionario()"
public abstract class Funcionario {
    private String nome;
    private double salario;
    
    public Funcionario(String nome) {
        this.nome = nome;
    }
    
    // Método abstrato: sem corpo (sem {}), obrigando as filhas a implementar
    public abstract double calcularBonus();

    // Getters e setters podem existir normalmente...
    public String getNome() { return nome; }
    public double getSalario() { return salario; }
    public void setSalario(double salario) { this.salario = salario; }
}

// Arquivo: Gerente.java
public class Gerente extends Funcionario {
    public Gerente(String nome) { super(nome); }

    @Override // Obrigado a implementar
    public double calcularBonus() {
        return getSalario() * 0.25;
    }
}

// Arquivo: Desenvolvedor.java
public class Desenvolvedor extends Funcionario {
    public Desenvolvedor(String nome) { super(nome); }

    @Override // Obrigado a implementar
    public double calcularBonus() {
        return getSalario() * 0.15;
    }
}
```

-----------------
### Interfaces (Adição)

Interfaces são "contratos" 100% abstratos. Elas definem **o que** uma classe deve fazer, mas nunca **como**. Uma classe pode `implement` (implementar) várias interfaces.

```java
// Arquivo: Autenticavel.java
// Uma interface define um "contrato" (ex: "sabe logar")
public interface Autenticavel {
    
    // Métodos de interface são 'public abstract' por padrão
    boolean logar(String senha);
    void deslogar();
}

// Arquivo: Gerente.java
// Gerente herda de Funcionario E implementa Autenticavel
public class Gerente extends Funcionario implements Autenticavel {
    
    private String senha;
    
    public Gerente(String nome) { 
        super(nome);
        this.senha = "123"; // Senha de exemplo
    }

    @Override // Do Funcionario
    public double calcularBonus() {
        return getSalario() * 0.25;
    }

    @Override // Da Interface Autenticavel (obrigatório implementar)
    public boolean logar(String senha) {
        if (this.senha.equals(senha)) {
            System.out.println(getNome() + " logado com sucesso.");
            return true;
        }
        System.out.println("Senha incorreta.");
        return false;
    }

    @Override // Da Interface Autenticavel (obrigatório implementar)
    public void deslogar() {
        System.out.println(getNome() + " deslogado.");
    }
}
```

-------
