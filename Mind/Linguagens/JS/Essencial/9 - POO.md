--------------

A partir do ES6, o JavaScript introduziu a sintaxe de `class` para facilitar a criação de objetos e a implementação de herança, tornando-a mais parecida com outras linguagens como Python ou Java.
### Classes e construtor

* `class`: A palavra-chave para definir uma classe.
* `constructor()`: Um método especial (similar ao `__init__` do Python) que é chamado automaticamente quando um novo objeto é criado (instanciado) com a palavra-chave `new`.
* `this`: Refere-se à instância atual do objeto (similar ao `self` do Python).

```javascript
class Pessoa {
    // O construtor inicializa os atributos do objeto
    constructor(nome, idade) {
        this.nome = nome;
        this.idade = idade;
    }

    // Métodos da classe
    saudar() {
        console.log(`Olá, meu nome é ${this.nome} e eu tenho ${this.idade} anos.`);
    }
}

// Para criar um objeto (instanciar), usamos a palavra 'new'
let p1 = new Pessoa("Kear", 25);
let p2 = new Pessoa("Ana", 30);

p1.saudar(); // Saída: Olá, meu nome é Kear e eu tenho 25 anos.
p2.saudar(); // Saída: Olá, meu nome é Ana e eu tenho 30 anos.


```javascript

```

------
### Herança (extends e super)

- `extends`: Usado para criar uma classe "filha" que herda de uma classe "mãe".
- `super()`: Usado **dentro do construtor** da classe filha para chamar o construtor da classe mãe. É obrigatório chamá-lo _antes_ de usar o `this` na classe filha.

```javascript
class Funcionario extends Pessoa {
    constructor(nome, idade, salario) {
        // 1. Chama o construtor da classe mãe (Pessoa)
        super(nome, idade);

        // 2. Adiciona os novos atributos
        this.salario = salario;
    }

    // A classe Funcionario herda o método saudar()
    // Mas também pode ter seus próprios métodos:
    trabalhar() {
        console.log(`${this.nome} está trabalhando com salário de ${this.salario}.`);
    }

    // Polimorfismo: Podemos sobrescrever um método da classe mãe
    saudar() {
        console.log(`Olá, sou o funcionário ${this.nome}.`);
    }
}

let func1 = new Funcionario("Carlos", 40, 5000);
func1.saudar();   // Saída: Olá, sou o funcionário Carlos.
func1.trabalhar(); // Saída: Carlos está trabalhando com salário de 5000.
```

-----------------
### Getters e Setters

Em JavaScript, `get` e `set` são palavras-chave especiais para criar métodos que parecem atributos. (Equivalente ao `@property` do Python).

- `get`: Usado para "ler" um valor.
- `set`: Usado para "definir" um valor, geralmente com alguma validação.

```javascript
class Produto {
    constructor(nome) {
        this._nome = nome; // Convenção de atributo "protegido"
    }

    // Getter: Permite ler "produto.nome" como se fosse um atributo
    get nome() {
        console.log("Chamou o GETTER");
        return this._nome.toUpperCase();
    }

    // Setter: Permite definir "produto.nome = 'novo'"
    set nome(novoNome) {
        console.log("Chamou o SETTER");
        if (novoNome.length > 2) {
            this._nome = novoNome;
        } else {
            console.log("Nome muito curto!");
        }
    }
}

let prod = new Produto("Livro");
console.log(prod.nome); // Chama o GETTER (Saída: LIVRO)

prod.nome = "CD"; // Chama o SETTER (Saída: Nome muito curto!)
prod.nome = "Monitor"; // Chama o SETTER
console.log(prod.nome); // Chama o GETTER (Saída: MONITOR)
```

-----------
### Métodos Estáticos (static)

Métodos estáticos (equivalentes ao `@staticmethod` ou `@classmethod` do Python) pertencem à **Classe** em si, e não à instância (objeto). Eles são chamados diretamente na classe, sem precisar usar `new`.

```javascript
class Matematica {
    static somar(a, b) {
        return a + b;
    }
}

// Não precisa criar uma instância: new Matematica()
let resultado = Matematica.somar(5, 10);
console.log(resultado); // Saída: 15
```

----------
### Encapsulamento (Privacidade)

- `_ (underline)`: Por convenção (assim como no Python), indica que um atributo ou método não deve ser acessado de fora da classe. (Ex: `this._nome`).
- `# (hash)`: (Moderno) Define um campo **realmente privado**. Tentar acessá-lo de fora da classe causará um erro.

```javascript
class Cofre {
    #senhaSecreta = 12345; // Campo privado

    constructor(nome) {
        this.nome = nome; // Campo público
    }

    verSenha() {
        console.log(this.#senhaSecreta); // Acesso permitido DENTRO da classe
    }
}

let meuCofre = new Cofre("Pessoal");
meuCofre.verSenha(); // Saída: 12345

// Tentar acessar de fora gera um erro:
// console.log(meuCofre.#senhaSecreta); // Erro!
```

-----------
