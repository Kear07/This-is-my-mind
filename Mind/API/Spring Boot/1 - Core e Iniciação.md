-------
### O que é o Spring Boot? E qual o problema que ele resolve?

Para entender o Spring Boot, você precisa entender o **Spring Framework** "clássico".

* **O Problema (Spring Clássico):** O Spring Framework é (e era) incrivelmente poderoso, mas exigia uma quantidade *gigantesca* de configuração manual. Para um projeto web simples, você precisava configurar o servidor (Tomcat), o despachante de requisições (DispatcherServlet), os visualizadores (JSP), o mapeamento de XML para beans, e dezenas de arquivos XML (o "XML Hell" ou Inferno do XML). Era complexo e demorado.

* **A Solução (Spring Boot):** O Spring Boot **não é** um novo framework. Ele é um "impulsionador" (Boot) para o Spring Framework. Sua principal função é **eliminar a configuração manual**.

Ele faz isso através de duas filosofias centrais.

------
### Conceitos fundamentais

#### O Spring Boot tem uma "opinião" forte sobre como uma aplicação moderna deve ser configurada.

* **Opinião:** "Você me disse que quer uma aplicação web? Então, *na minha opinião*, você vai querer usar o servidor Tomcat, vai querer o Jackson para lidar com JSON e vai querer configurar o Spring MVC desta forma específica."
* **Resultado:** Em vez de você ter que configurar tudo isso manualmente, o Spring Boot faz isso *automaticamente* para você. Você só precisa "discordar" (sobrescrever a configuração) se quiser algo diferente.

#### Auto-Configuração (`@EnableAutoConfiguration`)

Este é o cérebro mágico do Spring Boot.

1.  O Spring Boot olha o seu **classpath** (os arquivos `.jar` que você adicionou ao projeto).
2.  Ele vê, por exemplo, que você adicionou o `spring-boot-starter-web`.
3.  Ele pensa: "Opa, o `starter-web` está aqui, o que significa que o `spring-webmvc.jar` e o `tomcat-embed.jar` também estão. Isso *claramente* significa que o Kear quer rodar uma aplicação web."
4.  Com base nisso, ele **automaticamente** configura um `DispatcherServlet`, um servidor `Tomcat` embutido, e todo o resto necessário para uma API web funcionar.

Você não escreveu uma linha de configuração, e sua API já está pronta para rodar.

----------
### Os "Starters" (Entradas)

Os Starters são a principal ferramenta da "Auto-Configuração". Eles são pacotes `.jar` (dependências Maven/Gradle) que agrupam tudo o que você precisa para uma funcionalidade específica.

* `spring-boot-starter-web`: Puxa tudo o que é necessário para uma API REST (Tomcat, Spring MVC, Jackson JSON).
* `spring-boot-starter-data-jpa`: Puxa tudo para falar com um banco de dados (Hibernate, Spring Data, Conexão JDBC).
* `spring-boot-starter-security`: Puxa tudo para adicionar segurança (autenticação, filtros).
* `spring-boot-starter-test`: Puxa tudo para testes (JUnit 5, Mockito, AssertJ).

Você para de gerenciar 20 dependências individuais e passa a gerenciar apenas **um** Starter.

------------
### O Ponto de Partida: `@SpringBootApplication`

Todo projeto Spring Boot começa com uma classe `Main` simples, anotada com `@SpringBootApplication`.

```java
package com.kear.meuprojeto;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

// Esta anotação é a chave de tudo
@SpringBootApplication
public class MeuProjetoApplication {

    public static void main(String[] args) {
        // Esta linha "liga" o Spring Boot
        SpringApplication.run(MeuProjetoApplication.class, args);
    }
}
```

A "mágica" é que `@SpringBootApplication` é, na verdade, um atalho para três outras anotações:

1. **`@Configuration`**: Diz ao Spring: "Esta classe é uma fonte de definições de Beans" (veremos Beans no próximo arquivo).  
2. **`@EnableAutoConfiguration`**: O "cérebro" que discutimos acima. Liga a auto-configuração baseada no classpath.
3. **`@ComponentScan`**: Diz ao Spring: "Procure por outras classes anotadas (como `@Component`, `@Service`, `@RestController`) neste pacote (`com.kear.meuprojeto`) e em seus sub-pacotes, e as gerencie."

-------
### Configuração Externa: `application.properties`

Se a "Opinião" do Spring Boot não for o que você quer, você a sobrescreve aqui. Este arquivo vive em `src/main/resources`.
É um arquivo simples de chave-valor.

```java
# O Spring Boot usa o Tomcat na porta 8080 por padrão.
# Vamos mudar essa "opinião" para 9000.
server.port=9000

# Vamos definir o "context path" (prefixo) da nossa API
server.servlet.context-path=/api/v1

# Configuração de Banco de Dados (veremos em JPA)
spring.datasource.url=jdbc:postgresql://localhost:5432/meu_banco
spring.datasource.username=kear
spring.datasource.password=minhasenha123
```

**Alternativa (Recomendada): `application.yml`** O formato YAML é mais limpo e legível, pois é hierárquico.

```yaml
# A mesma configuração acima, mas em YAML
server:
  port: 9000
  servlet:
    context-path: /api/v1

spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/meu_banco
    username: kear
    password: minhasenha123
```

------

