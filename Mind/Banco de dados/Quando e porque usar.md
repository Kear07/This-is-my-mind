-----
### Bancos SQL (relacionais)

**Exemplos:** PostgreSQL, MySQL

**Características:**

- Modelo relacional (tabelas, linhas e colunas)
- Esquema bem definido
- Forte consistência (ACID)
- Excelente para transações

**Quando usar:**

- Sistemas financeiros
- Sistemas transacionais (OLTP)
- Regras de negócio complexas
- Relatórios e queries sofisticadas

---
### Bancos NoSQL

**Exemplos:** MongoDB, Redis, Neo4j

**Características:**

- Esquema flexível ou inexistente
- Alta escalabilidade horizontal
- Diferentes modelos de dados
- Eventual consistency (dependendo do banco)

**Quando usar:**

- Alto volume de dados
- Baixa latência
- Estruturas de dados dinâmicas
- Casos específicos (cache, grafos, documentos)

---
### Comparativo por tipo de banco

#### PostgreSQL

**Tipo:** Relacional (SQL)

**Use quando:**

- Consistência é crítica
- Necessidade de joins complexos
- Queries analíticas
- Regras de negócio no banco (constraints, triggers, functions)

**Evite quando:**

- Latência ultra baixa é prioridade absoluta
- Estrutura de dados extremamente variável

---

#### MySQL

**Tipo:** Relacional (SQL)

**Use quando:**

- Projetos legados ou compatibilidade de mercado
- Aplicações web tradicionais (LAMP)
- Ambientes simples de CRUD

**Observações:**

- Menos recursos avançados que o PostgreSQL
- Muito presente em hospedagens e sistemas antigos

---

#### MongoDB

**Tipo:** Documento (NoSQL)

**Use quando:**

- Estrutura de dados flexível
- Alto volume de leitura e escrita
- Evolução frequente do modelo de dados
- Logs, eventos, catálogos

**Evite quando:**

- Transações complexas entre múltiplos documentos
- Regras de consistência rígidas

---

#### Redis

**Tipo:** Key-Value (NoSQL)

**Use quando:**

- Cache
- Sessões de usuário
- Contadores
- Filas simples

**Evite quando:**

- Persistência de longo prazo
- Fonte única de verdade

---

#### Neo4j

**Tipo:** Grafo (NoSQL)

**Use quando:**

- Relações complexas

- Grafos sociais

- Recomendação

- Caminhos e dependências

**Evite quando:**

- CRUD simples

- Dados tabulares

---

### Casos de uso comuns

| Cenário                 | Banco recomendado  | Motivo                      |
| ----------------------- | ------------------ | --------------------------- |
| Sistema financeiro      | PostgreSQL         | ACID e consistência         |
| E-commerce              | PostgreSQL + Redis | Transações + cache          |
| Feed social             | MongoDB + Redis    | Flexibilidade + performance |
| Sistema de recomendação | Neo4j              | Relacionamentos complexos   |
| Logs e eventos          | MongoDB            | Esquema flexível            |

---

### Anti-patterns comuns

- Usar Redis como banco principal
- Usar MongoDB para transações financeiras
- Usar Neo4j para CRUD simples
- Usar SQL para dados altamente não estruturados

---

### Estratégia moderna (polyglot persistence)

Em sistemas reais, é comum usar mais de um banco:

- SQL como fonte de verdade
- Redis como cache
- MongoDB para dados flexíveis
- Neo4j para relações complexas

Essa abordagem é conhecida como **polyglot persistence**.

---

### Conclusão

A escolha do banco de dados deve ser guiada pelo problema, não pela tecnologia. Entender os trade-offs de cada opção é essencial para arquiteturas escaláveis, performáticas e sustentáveis.
