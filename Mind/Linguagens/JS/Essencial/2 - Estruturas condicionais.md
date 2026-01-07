------------------

Estruturas condicionais controlam o fluxo do código, executando blocos diferentes dependendo se uma condição é verdadeira (`true`) ou falsa (`false`).

### if / else
Usado para tomar uma decisão simples entre dois caminhos.

```javascript
let idade = 20;

if (idade >= 18) {
    console.log("Você é maior de idade.");
} else {
    console.log("Você é menor de idade.");
}
```

---------------------------------
### if / else if / else

Usado para tomar decisões entre múltiplos caminhos (mais de duas condições). O código testa as condições em ordem e executa o _primeiro_ bloco que for verdadeiro.

```javascript
let nota = 85;

if (nota >= 90) {
    console.log("Nota A: Excelente!");
} else if (nota >= 80) {
    console.log("Nota B: Muito Bom."); // Este bloco será executado
} else if (nota >= 70) {
    console.log("Nota C: Bom.");
} else {
    console.log("Nota D: Precisa melhorar.");
}
```

-----------
### Switch

Usado como alternativa ao `if / else if / else` quando você está comparando uma _única variável_ com múltiplos valores específicos (casos).

**Importante:**

- `case`: Define um valor específico para comparar.
- `break`: É **essencial** para parar a execução. Sem ele, o JavaScript continuará executando os `case` seguintes (comportamento "fall-through").
- `default`: É opcional e funciona como o `else`, executando se nenhum dos `case` for correspondido.

```javascript
let nivelDeAcesso = "ADMIN";

switch (nivelDeAcesso) {
    case "ADMIN":
        console.log("Acesso total permitido.");
        break;
    case "USUARIO":
        console.log("Acesso limitado ao painel.");
        break;
    case "VISITANTE":
        console.log("Acesso apenas à página inicial.");
        break;
    default:
        console.log("Nível de acesso desconhecido.");
}
```
