# API de Gerenciamento Clínico - Documentação Oficial

- [Visão Geral](#visão-geral)
- [Tecnologias](#tecnologias)
- [Instalação e Execução](#instalação-e-execução)
- [Base URL](#base-url)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Funcionalidades](#funcionalidades)
- [Autenticação e Autorização](#autenticação-e-autorização)
- [Endpoints](#endpoints)
- [Regras e Validações](#regras-e-validações)
- [Boas Práticas e Recomendações](#boas-práticas-e-recomendações)
- [Como Utilizar a API](#como-utilizar-a-api)
- [Contribuindo](#contribuindo)
- [Licença](#licença)
- [Contato](#contato)

---

## Visão Geral

Esta API foi desenvolvida como um **trabalho educacional pelo SENAI** com o objetivo de fornecer um sistema completo para o gerenciamento de clínicas médicas, contemplando funcionalidades essenciais como cadastro e controle de pacientes, médicos, consultas e usuários. A API foi implementada com **Java Spring Boot**, seguindo boas práticas de desenvolvimento, segurança e arquitetura RESTful.

O propósito principal é servir como base para aprendizado e desenvolvimento prático, permitindo que estudantes e profissionais compreendam e apliquem conceitos como autenticação JWT, criptografia de senhas, validações, e manipulação de dados em uma aplicação real.

---

## Tecnologias

- Java 21
- Spring Boot
- Spring Security (JWT)
- Spring Data JPA
- BCrypt
- Banco de dados relacional (PostgreSQL/MySQL)

---

## Instalação e Execução

1. **Pré-requisitos**
   - Java 21+
   - Maven 3.8+
   - Banco de dados relacional (ex: PostgreSQL/MySQL)

2. **Clone o repositório**
   ```bash
   git clone https://github.com/nyxpdb/Spring-Clinic
   cd Spring-Clinic
   ```

3. **Configure o banco de dados**
   - Edite o arquivo `application.properties` com as credenciais do seu banco.

4. **Build e execute**
   ```bash
   mvn spring-boot:run
   ```

---

## Base URL

```plaintext
http://localhost:8080
```

---

## Estrutura do Projeto

```plaintext
src/
 └── main/
      ├── java/
      │    └── com.clinic.management/
      │          ├── controller/                  # Controladores REST
      │          │     ├── ConsultaController.java
      │          │     ├── MedicoController.java
      │          │     ├── PacienteController.java
      │          │     └── UserController.java
      │          │
      │          ├── domain/                      # Entidades do domínio
      │          │     ├── Consulta.java
      │          │     ├── Medico.java
      │          │     ├── Paciente.java
      │          │     └── User.java
      │          │
      │          ├── enums/                       # Enumerações do sistema
      │          │     ├── TipoPermissao.java
      │          │     └── TipoUsuario.java
      │          │
      │          ├── dto/                         # Objetos de Transferência de Dados (DTOs)
      │          │     ├── ConsultaDTO.java
      │          │     ├── ConsultaUpdateDTO.java
      │          │     ├── MedicoDTO.java
      │          │     ├── MedicoUpdateDTO.java
      │          │     ├── PacienteDTO.java
      │          │     ├── PacienteUpdateDTO.java
      │          │     ├── UserDTO.java
      │          │     └── UserUpdateDTO.java
      │          │
      │          ├── exception/                   # Tratamento de exceções customizadas
      │          │     ├── GlobalExceptionHandler.java
      │          │     └── ResourceNotFoundException.java
      │          │
      │          ├── infra/                       # Infraestrutura (autenticação, segurança, etc)
      │          │     ├── auth/
      │          │     │    └── AuthService.java
      │          │     │
      │          │     ├── controller/
      │          │     │    └── AuthController.java
      │          │     │
      │          │     ├── dto/
      │          │     │    ├── LoginRequestDTO.java
      │          │     │    └── LoginResponseDTO.java
      │          │     │
      │          │     ├── domain/
      │          │     │
      │          │     ├── JwtConfig/
      │          │     │    ├── CustomUserDetailsService.java
      │          │     │    ├── JwtAuthenticationFilter.java
      │          │     │    └── SecurityConfig.java
      │          │     │
      │          │     └── service/
      │          │          └── jwt/
      │          │               └── JwtService.java
      │          │
      │          ├── mapper/                      # Mapeadores DTO  Entidades
      │          │     ├── ConsultaMapper.java
      │          │     ├── MedicoMapper.java
      │          │     ├── PacienteMapper.java
      │          │     └── UserMapper.java
      │          │
      │          ├── repository/                  # Repositórios Spring Data JPA
      │          │     ├── ConsultaRepository.java
      │          │     ├── MedicoRepository.java
      │          │     ├── PacienteRepository.java
      │          │     └── UserRepository.java
      │          │
      │          ├── response/                    # Classes para respostas padrão da API
      │          │     └── ApiResponse.java
      │          │
      │          └── service/                     # Serviços de negócio
      │                ├── ConsultaService.java
      │                ├── MedicoService.java
      │                ├── PacienteService.java
      │                └── UserService.java
```

---

## Funcionalidades

- **Gerenciamento de Pacientes**: Cadastro, consulta, atualização e exclusão.
- **Gerenciamento de Médicos**: Cadastro, consulta, atualização e exclusão.
- **Agendamento de Consultas**: Criação, listagem, busca por ID, filtro por médico e especialidade, cancelamento.
- **Gerenciamento de Usuários**: Cadastro e autenticação com segurança via JWT.
- **Autenticação e Autorização**: Login via JWT, proteção de rotas e controle de sessões.
- **Validações**: Validação de formatos de datas, campos obrigatórios e regras de negócio.
- **Segurança**: Criptografia de senhas via BCrypt, proteção de rotas sensíveis e controle de permissões por cargos.

---

## Autenticação e Autorização

A API utiliza **JSON Web Tokens (JWT)** para garantir segurança e controle de acesso às rotas protegidas.

- Para obter o token, faça uma requisição `POST /auth/login` com as credenciais válidas (`email` e `senha`).
- O token JWT retornado deve ser incluído no header `Authorization` de todas as requisições protegidas, no formato:

  ```
  Authorization: Bearer 
  ```

- O token possui validade padrão de **2 horas**. Após esse período, é necessário realizar novo login para obter um novo token.
- As permissões são controladas pelos cargos dos usuários, como **ADMIN**, **MÉDICO** e **PACIENTE**, definindo o nível de acesso e as ações permitidas.
- Endpoints públicos: `/auth/login` e cadastro de usuários.
- Endpoints protegidos: todos os demais, exigindo autenticação via JWT.
- O sistema utiliza **BCrypt** para armazenamento seguro das senhas, garantindo proteção contra ataques de força bruta.

---

## Endpoints

### Pacientes

| Método | Endpoint         | Descrição                   | Corpo da Requisição Exemplo                                                                                                 |
| ------ | ---------------- | --------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| GET    | `/patients`      | Lista todos os pacientes    | -                                                                                                                           |
| GET    | `/patients/{id}` | Busca paciente por ID       | -                                                                                                                           |
| POST   | `/patients`      | Cadastra novo paciente      | `{ "nome": "Paulo", "email": "paulo12@gmail.com", "telefone": "(11) 99034-2462", "dataNascimento": "2000-05-26" }`          |
| PUT    | `/patients/{id}` | Atualiza paciente existente | `{ "nome": "Paulo Ferreira", "email": "paulo14@gmail.com", "telefone": "(11) 09034-2462", "dataNascimento": "2000-05-26" }` |
| DELETE | `/patients/{id}` | Remove paciente por ID      | -                                                                                                                           |

### Médicos

| Método | Endpoint        | Descrição                | Corpo da Requisição Exemplo                                                                                                                               |
| ------ | --------------- | ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GET    | `/doctors`      | Lista todos os médicos   | -                                                                                                                                                         |
| GET    | `/doctors/{id}` | Busca médico por ID      | -                                                                                                                                                         |
| POST   | `/doctors`      | Cadastra novo médico     | `{ "nome": "Dr. Carlos Pereira", "especialidade": "Odontologia", "email": "carlos.pereira@clinic.com", "telefone": "(11) 91234-5678", "rm": "RM654321" }` |
| PUT    | `/doctors/{id}` | Atualiza dados do médico | `{ "nome": "Dr. André Pereira", "especialidade": "Odontologia", "email": "carlos.pereira@clinic.com", "telefone": "(11) 90868-5678", "rm": "RM654321" }`  |
| DELETE | `/doctors/{id}` | Remove médico por ID     | -                                                                                                                                                         |

### Consultas

| Método | Endpoint                              | Descrição                         | Corpo da Requisição Exemplo                                                                                 |
| ------ | ------------------------------------- | --------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| GET    | `/appointments`                       | Lista todas as consultas          | -                                                                                                           |
| GET    | `/appointments/{id}`                  | Busca consulta por ID             | -                                                                                                           |
| GET    | `/appointments/doctor/{doctorId}`     | Busca consultas por médico        | -                                                                                                           |
| GET    | `/appointments/specialty/{specialty}` | Busca consultas por especialidade | -                                                                                                           |
| POST   | `/appointments`                       | Agenda nova consulta              | `{ "idMedico": 1, "idPaciente": 2, "dataConsulta": "2025-06-02T15:50:00", "especialidade": "Odontologia" }` |
| DELETE | `/appointments/{id}`                  | Cancela/Remove consulta por ID    | -                                                                                                           |

### Usuários

| Método | Endpoint      | Descrição                         | Corpo da Requisição Exemplo                                                                                           |
| ------ | ------------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| POST   | `/auth/users`      | Cadastra novo usuário             | `{ "nome": "João Silva", "email": "joao@example.com", "senha": "123456", "tipoUsuario": "PACIENTE", "permissao": "ADMIN" }` |
| POST   | `/auth/login` | Autentica usuário e retorna token | `{ "email": "joao@example.com", "senha": "123456" }`                                                                  |

---

## Exemplos de Requisições

### Exemplo de autenticação

```bash
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"joao@example.com","senha":"123456"}'
```

### Exemplo de cadastro de paciente

```bash
curl -X POST http://localhost:8080/patients \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer " \
  -d '{"nome":"Maria Souza","email":"maria@example.com","telefone":"(11) 99999-9999","dataNascimento":"1990-04-15"}'
```

---

## Regras e Validações

- **Formato de Datas:**
  - Consultas: `YYYY-MM-DDTHH:mm:ss` (ISO 8601)
  - Data de nascimento: `YYYY-MM-DD`
- **Especialidades Válidas:**
  - `CARDIOLOGIA, DERMATOLOGIA, GINECOLOGIA, ORTOPEDIA, PEDIATRIA, ODONTOLOGIA`
- **Status de Consulta:**
  - `AGENDADA, CANCELADA, REALIZADA, REMARCADA`
- **Regras de Negócio:**
  - E-mail deve ser único para usuários, pacientes e médicos.
  - Senhas são armazenadas criptografadas usando BCrypt.
  - Consultas devem ser agendadas dentro do horário permitido e sem conflito de agenda.

---

## Boas Práticas e Recomendações

- Utilize sempre HTTPS em ambientes de produção para garantir a segurança dos dados em trânsito.
- Realize backups periódicos do banco de dados.
- Controle o acesso ao banco de dados com usuários específicos e permissões mínimas.
- Monitore logs de acesso e erros para identificar possíveis ataques ou problemas na aplicação.
- Considere implementar rate limiting para evitar ataques de força bruta na autenticação.

---

## Como Utilizar a API

1. **Cadastro**
   - Cadastre pacientes, médicos e usuários usando os endpoints correspondentes (`POST`).

2. **Autenticação**
   - Envie as credenciais (`email` e `senha`) para `/auth/login` para obter o token JWT.

3. **Requisições Autenticadas**
   - Use o token JWT retornado no header `Authorization` em formato:

     ```
     Authorization: Bearer 
     ```

   - Apenas rotas protegidas aceitam o token para autorização.

---

## Contribuindo

Contribuições são bem-vindas! Para contribuir:

- Fork este repositório
- Crie uma branch (`git checkout -b feature/nova-funcionalidade`)
- Commit suas alterações (`git commit -m 'Adiciona nova funcionalidade'`)
- Push para a branch (`git push origin feature/nova-funcionalidade`)
- Abra um Pull Request

---

## Licença

Este projeto está licenciado sob a licença MIT.

---

## Contato

Para dúvidas, sugestões ou contribuições, entre em contato com a equipe de desenvolvimento.

---
