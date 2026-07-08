# Sistema de Gestão de Provedora de Internet (SGPI API)

API REST desenvolvida em Java com Spring Boot para gerenciamento de uma provedora de internet por fibra óptica.

---

# Tecnologias utilizadas

- Java 17
- Spring Boot
- Spring Data JPA
- PostgreSQL 16
- Docker
- Docker Compose
- pgAdmin 4
- ModelMapper
- Lombok
- SpringDoc OpenAPI (Swagger)

---

# Clonando o projeto

```bash
git clone <url-do-repositorio>

cd sgpiapi
```

---

# Estrutura do Docker

O projeto utiliza dois containers.

| Container | Função |
|-----------|--------|
| PostgreSQL | Banco de dados |
| pgAdmin | Administração do banco |

O arquivo docker-compose.yml cria ambos automaticamente.

---

# Iniciando os containers

Na pasta do projeto execute:

```bash
docker compose up -d
```

Verifique se estão executando:

```bash
docker ps
```

Resultado esperado:

```
spring-postgres
spring-pgadmin
```

---

# Banco de dados

O banco é criado automaticamente.

Configuração padrão:

Banco:

```
sgpiapi
```

Usuário:

```
postgres
```

Senha:

```
postgres
```

Porta:

```
5432
```

---

# Acessando o PostgreSQL pelo terminal

Entrar no container:

```bash
sudo docker exec -it spring-postgres bash
```

ou diretamente no psql:

```bash
sudo docker exec -it spring-postgres psql -U postgres -d sgpiapi
```

---

# Acessando o pgAdmin

Abrir:

```
http://localhost:5050
```

Login:

Email

```
admin@admin.com
```

Senha

```
admin
```

---

# Criando o servidor no pgAdmin

Após entrar no pgAdmin:

Clique em

```
Servers
```

Depois

```
Create
```

```
Server
```

### Aba General

Nome:

```
PostgreSQL Local
```

### Aba Connection

Host:

```
postgres
```

Caso não funcione:

```
host.docker.internal
```

ou

```
localhost
```

(Quando o pgAdmin também estiver em Docker normalmente utiliza-se **postgres**, que é o nome do serviço no docker-compose.)

Porta

```
5432
```

Maintenance Database

```
sgpiapi
```

Username

```
postgres
```

Password

```
postgres
```

Marque

```
Save Password
```

Clique em Save.

---

# Onde encontrar as tabelas

Após conectar:

```
Servers
```

↓

```
PostgreSQL Local
```

↓

```
Databases
```

↓

```
sgpiapi
```

↓

```
Schemas
```

↓

```
public
```

↓

```
Tables
```

Ali estarão todas as tabelas do sistema.

---

# Consultando dados no pgAdmin

Clique com botão direito na tabela.

Depois:

```
View/Edit Data
```

↓

```
All Rows
```

ou utilize o Query Tool.

Exemplo:

```sql
SELECT * FROM regiao;
```

---

# Configuração do Spring

application.properties

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/sgpiapi
spring.datasource.username=postgres
spring.datasource.password=postgres

spring.jpa.hibernate.ddl-auto=update

spring.jpa.show-sql=true

springdoc.swagger-ui.path=/swagger-ui.html
```

---

# Executando o projeto

Utilizando Maven Wrapper:

```bash
./mvnw spring-boot:run
```

ou

```bash
./mvnw clean package

java -jar target/*.jar
```

---

# Swagger

Após iniciar o projeto:

```
http://localhost:8080/swagger-ui/index.html
```

ou

```
http://localhost:8080/swagger-ui.html
```

Caso a autenticação esteja desabilitada, todas as rotas poderão ser testadas diretamente.

---

# Primeiros cadastros recomendados

Algumas tabelas são de apoio e devem possuir registros antes da utilização das demais funcionalidades.

## Tipo de Movimentação

Necessário para movimentação de estoque.

| Nome | Operação |
|--------|----------|
| Entrada | E |
| Saída | S |

---

## Regiões

Exemplo:

| Nome | Sigla |
|-------|--------|
| Centro | CTR |
| Norte | NOR |
| Sul | SUL |

---

## Datas de vencimento

Exemplos:

| Dia |
|------|
| 5 |
| 10 |
| 15 |
| 20 |

---

## Prioridades

| Nome |
|-------|
| Baixa |
| Média |
| Alta |

---

## Situação do Cliente

Exemplo:

- Ativo
- Bloqueado
- Cancelado

---

## Situação do Contrato

Exemplo:

- Ativo
- Suspenso
- Cancelado

---

## Situação do Usuário

Exemplo:

- Ativo
- Inativo

---

## Status da Ordem de Serviço

Exemplo:

- Aberta
- Em andamento
- Concluída
- Cancelada

---

## Diagnóstico Inicial

Exemplos:

- Sem conexão
- Lentidão
- Equipamento sem energia

---

## Diagnóstico Final

Exemplos:

- Cabo rompido
- Configuração corrigida
- Equipamento substituído

---

## Tipo de Atendimento

Exemplos:

- Presencial
- Telefone
- WhatsApp

---

## Status do Atendimento

Exemplos:

- Aberto
- Em atendimento
- Finalizado

---

## Unidade de Medida

Exemplos:

| Nome | Sigla |
|--------|--------|
| Unidade | UN |
| Metro | M |
| Caixa | CX |

---

## Período de Atendimento

Exemplos:

- Manhã
- Tarde
- Noite

---

## Plano de Internet

Exemplos:

| Nome | Velocidade |
|--------|------------|
| Básico | 300 Mbps |
| Intermediário | 600 Mbps |
| Premium | 1 Gbps |

---

## Material

Cadastrar alguns materiais para testes.

Exemplo:

| Nome | Estoque |
|--------|----------|
| Conector SC/APC | 100 |
| Cabo Drop | 500 |
| ONU Huawei | 20 |

---

# Fluxo recomendado para testes

1. Cadastrar Tipo de Movimentação
2. Cadastrar Unidade de Medida
3. Cadastrar Material
4. Realizar Entrada de Estoque
5. Realizar Saída de Estoque
6. Verificar atualização do estoque
7. Cadastrar Regiões
8. Cadastrar Clientes
9. Cadastrar Endereços
10. Cadastrar Contratos
11. Criar Atendimentos
12. Criar Ordens de Serviço

---

# Observações

O projeto ainda está em desenvolvimento. A autenticação JWT será implementada após a conclusão e validação das funcionalidades CRUD de todas as entidades.
