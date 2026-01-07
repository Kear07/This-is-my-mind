-------------
### O Problema: Um Código, Múltiplos Ambientes

Você nunca deve rodar sua aplicação em Produção com as mesmas configurações de Desenvolvimento.
* **Em DEV:** Você quer se conectar a um banco `localhost`, usar `ddl-auto: update` e talvez ter logs mais detalhados.
* **Em PROD:** Você PRECISA se conectar ao banco de dados real (na nuvem), usar `ddl-auto: validate` ou `none`, e ter chaves de API secretas.

Como gerenciar isso sem mudar o código? A resposta é: **Profiles (Perfis)**.

------------
### Spring Profiles

Profiles são uma forma de "agrupar" configurações em arquivos separados. O Spring Boot carregará o `application.properties` (ou `.yml`) padrão e, em seguida, sobrescreverá as propriedades com as do perfil que estiver *ativo*.

#### a) Estrutura de Arquivos

A forma mais limpa é criar um arquivo para cada ambiente em `src/main/resources/`:

* **`application.properties`**
    * (Contém configurações COMUNS a todos os ambientes)
    ```java
    # Porta padrão para todos
    server.port=8080
    # Nome da aplicação
    spring.application.name=minha-api
    ```

* **`application-dev.properties`**
    * (Configurações *apenas* para Desenvolvimento)
    ```java
    # Sobrescreve a URL do banco
    spring.datasource.url=jdbc:postgresql://localhost:5432/db_dev
    spring.datasource.username=postgres
    spring.datasource.password=1234
    
    # DDL-auto é seguro em dev
    spring.jpa.hibernate.ddl-auto=update
    spring.jpa.show-sql=true
    ```

* **`application-prod.properties`**
    * (Configurações *apenas* para Produção)
    ```java
    # Aponta para o banco de produção (ex: na AWS RDS)
    spring.datasource.url=jdbc:postgresql://[minha-db.rds.amazonaws.com/db_prod](https://minha-db.rds.amazonaws.com/db_prod)
    
    # NUNCA use senhas direto aqui! Use Variáveis de Ambiente
    spring.datasource.username=${DB_USER}
    spring.datasource.password=${DB_PASSWORD}
    
    # DDL-auto é perigoso em prod
    spring.jpa.hibernate.ddl-auto=validate
    spring.jpa.show-sql=false
    ```

#### b) Como Ativar um Perfil

Você diz ao Spring qual perfil carregar na hora de *executar* o `.jar`.

**Via Linha de Comando (mais comum):**
```bash
# Executa o JAR ativando o perfil 'prod'
# O Spring vai carregar 'application.properties' + 'application-prod.properties'
java -jar minha-api.jar --spring.profiles.active=prod
```

Via Variável de Ambiente:

```java
export SPRING_PROFILES_ACTIVE=prod
java -jar minha-api.jar
```

----------
### Spring Boot Actuator (Monitoramento)

O Actuator é um "starter" que expõe endpoints de gerenciamento e monitoramento sobre sua própria aplicação. Ele é a "caixa de fusíveis" da sua API.

**1. Adicione a dependência:** `spring-boot-starter-actuator`
**2. Configure o `application.properties`:** Por segurança, o Actuator não expõe tudo pela web por padrão. Você precisa habilitar.
```java
# Em /actuator, expõe os endpoints 'health' e 'info'
management.endpoints.web.exposure.include=health,info,metrics,prometheus
```

**3. Endpoints Essenciais:** (Se sua API roda em `localhost:8080`, o Actuator roda em `localhost:8080/actuator`)

- **`/actuator/health`** (O mais importante)
    
    - Retorna um JSON simples: `{"status": "UP"}`.
    - É usado por ferramentas de orquestração (como Kubernetes ou Load Balancers) para saber se sua aplicação está "viva". Se ela não responder "UP", o orquestrador para de enviar tráfego para ela e a reinicia.
    - Ele automaticamente verifica a conexão com o banco (DataSource), espaço em disco, etc.
    
- **`/actuator/info`** 
    - Mostra informações estáticas que você pode definir, como a versão do build.
    
- **`/actuator/metrics`**
    
    - Mostra métricas detalhadas (uso de CPU, memória, threads, etc.).
    
- **`/actuator/prometheus`**
    
    - Expõe as métricas no formato específico para a ferramenta de monitoramento **Prometheus**, que é o padrão de mercado hoje.

-----------------
### "Dockerizando" a Aplicação

Para rodar sua API em produção (ex: na nuvem), você a colocará em um contêiner Docker.

Crie um arquivo chamado `Dockerfile` na raiz do seu projeto:

```java
### ESTÁGIO 1: Build (Construção)
# Usamos uma imagem do Java (JDK) que já tem o Maven (ferramenta de build)
FROM maven:3.8-openjdk-17 AS build

# Define o diretório de trabalho dentro do contêiner
WORKDIR /app

# Copia os arquivos de configuração do build
COPY pom.xml .

# Copia o código-fonte
COPY src ./src

# Executa o build do Maven.
# "-DskipTests" pula os testes (boa prática é testar em um pipeline de CI antes)
# "package" gera o arquivo .jar final
RUN mvn package -DskipTests

### ESTÁGIO 2: Run (Execução)
# (Multi-stage build)
# Agora, usamos uma imagem MUITO MENOR, apenas com o Java (JRE)
# Isso torna nossa imagem final leve e segura.
FROM openjdk:17-jre-slim

# Define o diretório de trabalho
WORKDIR /app

# Copia APENAS o .jar que foi gerado no EstágIO 1
# (O nome do .jar deve bater com o que está no seu pom.xml)
COPY --from=build /app/target/minha-api-0.0.1-SNAPSHOT.jar app.jar

# Expõe a porta que a aplicação Spring (Tomcat) usa
EXPOSE 8080

# Comando para executar a aplicação quando o contêiner iniciar
# Aqui é onde ativamos o perfil de produção!
ENTRYPOINT ["java", "-jar", "app.jar", "--spring.profiles.active=prod"]
```

Para construir e rodar (localmente):

```java
# 1. Constrói a imagem Docker
docker build -t minha-api-imagem .

# 2. Roda o contêiner
# -p 8080:8080 -> Mapeia a porta 8080 do seu PC para a 8080 do contêiner
# -e ... -> Define as Variáveis de Ambiente que o 'application-prod.properties' espera
docker run -p 8080:8080 \
  -e DB_USER="usuario_prod" \
  -e DB_PASSWORD="senha_prod" \
  minha-api-imagem
```